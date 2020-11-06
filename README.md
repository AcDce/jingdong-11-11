let MAX_EPOCH = 128 //最大执行次数
try {
    auto();
} catch (error) {
    toast("请手动开启无障碍并授权给Auto.js"); sleep(2000); exit();
}
//console.show()

function btn_click(x) { if (x) x.click() }

//获取对应的任务按钮
function get_task(pat) {
    let x = textContains("邀请好友").findOne(5000)
    if (!x) {
        console.warn('网络卡顿，无法返回到任务列表界面')
        back(); return null
    }
    list_x = x.parent().children()
    for (let i = 0; i < list_x.length; i++) {
        txt = list_x[i].text()
        if (txt.indexOf(pat) > -1 && list_x[i + 1].text() != "已完成") {
            return list_x[i + 1]
        }
    }
    return null
}

//浏览5个商品
function browse_five_goods_task() {
    sleep(1500)
    list_money = textStartsWith('¥').find()
    for (let ii = 0; ii < list_money.length && ii < 5; ii++) {
        x = list_money[ii]
        list_btn = x.parent().parent().children()
        for (let i = 0; i < list_btn.length; i++) {
            if (list_btn[i].clickable()) {
                list_btn[i].click();
                sleep(1000); back(); sleep(200);
            }
        }
    }
    back()
}

//加购5个商品
function purchase_five_goods_task() {
    sleep(2000)
    list_money = idContains("jmdd-react-smash").find()
    for (let i = 0; i < 5 && i < list_money.length; i++) {
        list_money[i].click()
        sleep(800)
    }
    back()
}

//执行加购5个商品
function do_purchase_five_goods_task() {
    console.log('开始执行加购5个商品')
    for (let i = 0; i < MAX_EPOCH; i++) {
        let btn_todo = get_task("加购5个商品")
        if (!btn_todo) break
        btn_todo.click()
        purchase_five_goods_task()
    }
    console.log('**加购5个商品任务，完成')
}

//执行浏览5个商品
function do_browse_five_goods_task() {
    console.log('开始执行浏览5个商品任务')
    for (let i = 0; i < MAX_EPOCH; i++) {
        let btn_todo = get_task("浏览5个商品")
        if (!btn_todo) break
        btn_todo.click()
        browse_five_goods_task()
    }
    console.log('**浏览5个商品任务,完成')
}

//执行简单8秒任务
function do_8sec() {
    console.log('开始执行简单8秒任务')
    for (let i = 0; i < MAX_EPOCH; i++) {
        let btn_todo = get_task("8秒")
        if (!btn_todo) break
        btn_todo.click()
        sleep(Math.ceil(Math.random() * 3000) + 12000); back();
    }
    console.log('**执行简单8秒任务，完成')
}

//执行精选联合会员任务
function do_vip() {
    console.log('开始执行精选联合会员任务')
    for (let i = 0; i < MAX_EPOCH; i++) {
        let btn_todo = get_task("成功入会")
        if (!btn_todo) break
        btn_todo.click()
        sleep(3000)
        btn_assure = textContains('确认授权并加入店铺会员').findOne(2000)
        if (!btn_assure) continue
        btn_assure.click()
        sleep(3000)
        back()
    }
    console.log('精选联合会员任务，完成')
}

//执行简单浏览任务(无时间限制)
function do_simple_task() {
    console.log('开始执行简单浏览任务(无时间限制)')
    for (let i = 0; i < MAX_EPOCH; i++) {
        let btn_todo = get_task("浏览可得")
        if (!btn_todo) break
        btn_todo.click()
        sleep(1000); back();
        btn_click(textContains('离开').findOne(1000))
    }
    console.log('简单浏览任务(无时间限制)，完成')
}

function do_all_task() {
    do_8sec()
    do_browse_five_goods_task()
    do_purchase_five_goods_task()
    do_vip()
    do_simple_task()
}


//等待用户进入活动主界面
while (true) {
    let btn_get = text("领金币").findOne(2000)
    if (btn_get) {
        btn_get.click(); break;

    }
    console.log('程序启动成功，等待用户进入京东活动主界面')
}

let options = dialogs.multiChoice("请选择需要执行的任务", ['浏览8秒任务', '浏览5个商品', '加购5个商品', '联合会员', '简单任务(无时间限制)', '全部任务'])

options.forEach(option => {
    switch (option) {
        case 0:  //执行浏览8秒任务
            do_8sec(); break;
        case 1: //浏览5个商品
            do_browse_five_goods_task(); break;
        case 2: //加购5个商品
            do_purchase_five_goods_task(); break;
        case 3: //联合会员
            do_vip(); break;
        case 4: //简单任务
            do_simple_task(); break;
        case 5: //全部任务
            do_all_task(); break;
    }
});
