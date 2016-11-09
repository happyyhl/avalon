# avalon
avalon2
#    记 笔记 

#######################################
在xshell里写代码：
例如 vi time.py 进入vi编辑界面 
保存工作并退出 vi编辑界面，无论是否退出 vi编辑界面，均可保存代码。
按 ESC 键，确定 vi 是否处于命令模式。

输入 i 可键入

保存，但不退出 vi
:w

保存并退出 vi
:wq

退出 vi，但不保存更 改
:q

用其他文件名保存
:w filename
 
在现有文件中保存并覆盖原文件
:w! filename
#######################################

#注意点
## 1:domReady后如何扫描
        $(function(){
            var vm = avalon.define({/* */});
             //如果你将vm定义在jQuery的ready方法内部,那么avalon的扫描就会失效,需要手动扫描
            avalon.scan(document.body) 
        })
## 2:直接提交 avalon 对象
        JSON.parse(JSON.stringify(vm.data.$model))
## 3：不能将vm中的数组或子对象取出来,再用它们赋给vm的某个数组或子对象,
        vm.arr2 = vm.arr1 //报错
        vm.arr2 = vm.arr1.$model //正常
## 4:在使用日期过滤器 时候 
        <td><div>{{ el.startTime | date("yyyy-MM-dd HH:mm:ss") }}</div></td> //运用时错误
        
        var common = {
            dateFormatStr:"yyyy-MM-dd HH:mm:ss"
        };
        <td><div>{{ el.startTime | date(common.dateFormatStr) }}</div></td> //正常
## 5:我们可以在首页渲染页面时, 想挡住双花括号乱码问题,可以尝试这样干
         .ms-controller{
             visibility: hidden;
          }
## 6:关闭 控制台打印消息
         avalon.config({
            debug: false
         })
#问题总结
## 1:checkbox 勾选问题
        var vm=avalon.define({
            showList:[] //当前显示题目列表
        });
        //重新赋值 需要先清空 不然原先的 input可能会被勾选上
        vm.showList=[];
        vm.showList = data.resultObject.items;
        //
        <div ms-for="el in @showList">
            <p width="30" class="tdborder"><input type="checkbox" class="chk_box" ms-attr="{value:el.id}" /></p>
        </div>
        
## 2:关键字问题（ie兼容）
        //ms-attr 不能有 class 等某些（未知）关键字；
        <a class="btns1 frontcls60bj10" ms-attr="{paperId:el.paperId,class:'aa'}"></a> // 错误
        
## 3:ms-if （IE兼容）
         ms-if="el.testStatus >=1"  ||  ms-if="el.testStatus <=1" 
         //最好用 ms-visible  代替 否则可能 会出现显示不了的 情况；原因未知；

###选择框方法
        $("input[type='checkbox']").not(":checked");$("input[type='checkbox']").is(":checked");

