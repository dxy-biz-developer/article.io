title: 丁香园（E-marketing项目使用技巧,持续更新中）
date: 2016-04-13 09:10:59
tags:
---

本文主要是我入职以来在E-marketing项目中常用到的一些控件使用，兼容方法，交互效果的说明和使用方法，欢迎大家对本文进行更改或者补充，下文中所有`require()`的类库均可在各个E-marketing项目中找到，可以直接copy到自己项目中使用。
# 目录
1. *[判断是否IE8](#判断是否IE8)*
2. *[IE8修复placeholder](#IE8修复placeholder)*
3. *[IE8 box-shaadow兼容](#IE8 box-shadow兼容)*
4. *[IE8 rgba兼容](#IE8 rgba兼容)*
5. *[分享组件使用](#分享组件使用)*
6. *[地址选择控件](#地址选择控件)*
7. *[医院选择控件](#医院选择控件)*
8. *[科室选择控件](#科室选择控件)*
9. *[职称选择控件](#职称选择控件)*
10. *[判断是否移动端](#判断是否移动端)*
11. *[获取内页文章ID](#获取内页文章ID)*
12. *[validate插件扩展](#validate插件扩展)*
13. *[validate插件使用](#validate插件使用)*
14. *[bxslider轮播图使用](#bxslider轮播图使用)*
15. *[一般选项卡](#一般选项卡)*
16. *[新窗口打开页面](#新窗口打开页面)*
17. *[滚动到指定位置滑动动画](#滚动到指定位置滑动动画)*
18. *[对话框控件使用](#对话框控件使用)*
19. *[移动端PPT使用](#移动端PPT使用)*




## <a name="判断是否IE8"></a>1.判断是否IE8
	```js
	var isIE=!!window.ActiveXObject; 
	var isIE8=isIE&&!!document.documentMode;
    ```
## <a name="IE8修复placeholder"></a>2.IE8修复placeholder
	```js
	//placeholder兼容
	var JPlaceHolder = {
    //检测
    _check : function(){
        return 'placeholder' in document.createElement('input');
    },
    //初始化
    init : function(){
        if(!this._check()){
            this.fix();
        }
    },
    //修复
    fix : function(){
        jQuery(':input[placeholder]').each(function(index, element) {
            var self = $(this), txt = self.attr('placeholder');
            self.attr('placeholder','');
            self.wrap($('<div></div>').css({position:'relative', zoom:'1', border:'none', background:'none', padding:'none', margin:'none'}));
            var pos = self.position(), h = self.outerHeight(true), paddingleft = self.css('padding-left');
            var holder = $('<span></span>').text(txt).css({position:'absolute', left:pos.left, top:pos.top, height:h, lineHeight:h+'px', paddingLeft:paddingleft, color:'#aaa',width:'100%',display:'block'}).appendTo(self.parent());
            self.focus(function(e) {
                holder.hide();
            }).blur(function(e) {
                if(!self.val()){
                    holder.show();
                }
            });
            holder.click(function(e) {
                holder.hide();
                self.focus();
            });
        });
    }
	};
	//执行
		jQuery(function(){
    	JPlaceHolder.init();    
	});
	```
## <a name="IE8 box-shadow兼容"></a>3.IE8 box-shadow兼容
	```js
	//如果IE8，则添加shaded class
	.shaded { 
	    background-color: #fff;
	    zoom: 1; 
	    filter: progid:DXImageTransform.Microsoft.Shadow(color='#aaaaaa', Direction=135, Strength=6);
	}	
	```
## <a name="IE8 rgba兼容"></a>4.IE8 rgba兼容
	```html
	//其格式为 #AARRGGBB 。 AA 、 RR 、 GG 、 BB 为十六进制正整数。取值范围为 00 - FF 。 RR 指定红色值， GG 指定绿色值， BB 指定蓝色值。
	//参阅 #RRGGBB 颜色单位。 AA 指定透明度。 00 是完全透明。 FF 是完全不透明。超出取值范围的值将被恢复为默认值。
	
	filter:progid:DXImageTransform.Microsoft.gradient(startColorstr=#7f000000,endColorstr=#7f000000);
	```
## <a name="分享组件使用"></a>5.分享组件使用

	```js
	
	var share = require('./dxy_share.js');
	
	var shareContainer = $('放置容器'); //给一个放置容器，其他事情控件会帮你完成
	    if (shareContainer.length) {
	        shareContainer.attr('id','to-share');
	        new share().show({
	            id: 'to-share',
	            lst: ['weixin', 'sina', 'tt', 'qzone', 'douban'],
	            style: 3
	        });
	}
    //sina: '新浪微博',
    //tt: '腾讯微博',
    //douban: '豆瓣',
    //renren: '人人网',
    //qzone: 'QQ空间',
    //idxy: '丁香客',
    //kaixin: '开心网',
    //weixin: '微信',
    //favorite: '收藏夹'
	```

现在已经被范同学优化更新，详细使用方法请见[dxy-share](https://github.com/dxy-biz-developer/dxy-share)	
## <a name="地址选择控件"></a>6.地址选择控件
	```js
	var Cascading = require('../lib/Cascading.full.js');
	
	//桌面版地址
	        cascadeLocation = new Cascading.Desktop({
	            type: 'common',
	            data: 'dataLocation',
	            dataUrl: 'http://assets.dxycdn.com/core/widgets/cascading-list-v2/data/location.js?t=123',
	            container: 'cascading-location-container',
	            additionClass: 'test',
	            maxLevel: 3,
	            maxSelect: 1,
	            ieFallback: true,
	            panelNames: ['省份', '城市', '区县'],
	            defaultSelected: [['150000', '150600', '150621'], ['130000', '130200', '130281']],
	            confirmCallback: function(data){
	                $('#location').val(data.pathName[0].join('>'));//$('#location')为你的input输入框
	            }
        });

        $('#location').on('focus', function(e){
            cascadeLocation.open();
            this.blur();
            
        });
		//移动版地址
	        cascadeLocationMobile = new Cascading.Mobile({
	            type: 'common',
	            data: 'dataLocation',
	            dataUrl: 'http://assets.dxycdn.com/core/widgets/cascading-list-v2/data/location.js?t=123',
	            container: 'cascading-location-mobile-container',
	            additionClass: 'test',
	            maxLevel: 3,
	            maxSelect: 1,
	            ieFallback: true,
	            panelNames: ['省份', '城市', '区县'],
	            defaultSelected: [['150000', '150600', '150621'], ['130000', '130200', '130281']],
	            confirmCallback: function(data){
	                $('#location').val(data.pathName[0].join('>'));//$('#location')为你的input输入框
	            }
        });

        $('#location').on('focus', function(e){
            cascadeLocation.open();
            this.blur();
            
        });	
	```
## <a name="医院选择控件"></a>7.医院选择控件
	```js
	// 桌面版医院
        cascadeHospital = new Cascading.Desktop({
            type: 'hospital',
            data: 'dataLocation2Level',
            dataUrl: 'http://assets.dxycdn.com/core/widgets/cascading-list-v2/data/location_2level.js?t=123',
            panelNames: ['等级', '医院'],
            panelWidth: ['30', '70'],
            container: 'cascading-hospital-container',
             defaultSelected: [['150000', '150600']],
            ieFallback: true,
            confirmCallback: function(data){
                $('#hospital-box').val(data.name.join(',')); //$('#hospital-box')为你的input输入框
            }
        });
        $('#hospital-box').on('focus', function(e){ 
            cascadeHospital.open();
            this.blur();
        });
    //移动版医院
    	cascadeHospitalMobile = new Cascading.Mobile({
            type: 'hospital',
            container: 'cascading-hospital-mobile-container',
            additionClass: 'test',
            data: 'dataLocation2Level',
            dataUrl: 'http://assets.dxycdn.com/core/widgets/cascading-list-v2/data/location_2level.js?t=123',
            panelNames: ['等级', '医院'],
            panelWidth: ['30', '70'],
             defaultSelected: [['150000', '150600']],
            ieFallback: true,
            confirmCallback: function(data){
                $('#hospital-box').val(data.name.join(','));
            },
            cancelCallback: function(){
            }
        });

        $('#hospital-box').on('click', function(e){
            cascadeHospitalMobile.open();
            this.blur();
        });    
	```

## <a name="科室选择控件"></a>8.科室选择控件
	```js
	// 桌面版科室
        cascadeDepartment = new Cascading.Desktop({
            type: 'department',
            data: 'dataDivision',
            dataUrl: 'http://assets.dxycdn.com/core/widgets/cascading-list-v2/data/division.js',
            container: 'cascading-department-container',
            onlyLast: true,
            additionClass: 'cascading-department-container',
            maxSelect: 1,
            ieFallback: true,
            panelNames: ['科室', '科室二级'],
            confirmCallback: function(data){
                $('#department-box').val(data.pathName.join('>'));//$('#department-box')为你的input输入框
            }
        });

        $('#department-box').on('focus', function(e){
            cascadeDepartment.open();
            this.blur();
        });
	// 移动版科室
        cascadeLocationMobile = new Cascading.Mobile({
            type: 'common',
            data: 'dataDivision',
            dataUrl: 'http://assets.dxycdn.com/core/widgets/cascading-list-v2/data/division.js',
            container: 'cascading-depart-mobile-container',
            confirmCallback: function(data){                    
                $('#department-box').val(data.pathName.join(','));
            },
            cancelCallback: function(){

            }
        });

        $('#department-box').on('click', function(e){
            cascadeLocationMobile.open();
            this.blur();
        });
	```
## <a name="职称选择控件"></a>9.职称选择控件
	```js
	        // 桌面版职称
        cascadeTitle = new Cascading.Desktop({
            type: 'common',
            data: 'dataTitle',
            dataUrl: 'http://assets.dxycdn.com/core/widgets/cascading-list-v2/data/title.js',
            container: 'cascading-title-container',
            additionClass: 'test',
            ieFallback: true,
            confirmCallback: function(data){
                $('.job-input').val(data.pathName[0].join('>'));
                

            }
        });

        $('.job-input').on('focus', function(e){
            cascadeTitle.open();
            this.blur();
        });
	    // 移动版职称
        cascadeTitleMobile = new Cascading.Mobile({
            type: 'common',
            data: 'dataTitle',
            dataUrl: 'http://assets.dxycdn.com/core/widgets/cascading-list-v2/data/title.js',
            container: 'cascading-title-mobile-container',
            additionClass: 'test',
            ieFallback: true,
            confirmCallback: function(data){
                $('.job-input').val(data.pathName[0].join('>'));

            }
        });

        $('.job-input').on('focus', function(e){
            cascadeTitle.open();
            this.blur();
        });
	```

## <a name="判断是否移动端"></a>10.判断是否移动端
	```js
		var isMobile = /mobile|dxyapp/i.test(window.navigator.userAgent);
	```
## <a name="获取内页文章ID"></a>11.获取内页文章ID
	```js
	function getContentId() {
    	var match = $('body').attr('class').match(/page-node-(\d+)/);
    	return match ? match[1] : -1;
	};
	```
## <a name="validate插件扩展"></a>12.validate插件扩展
	```js
	//将该段代码写在validate.min.js最后来增加自定义扩展
	jQuery.extend(jQuery.validator.messages, {
	    required: "此项必填",
	    chineseName: "请输入正确的中文名",
	    mobileNumber: "请输入正确的手机号码",
	    remote: "请重新输入",
	    email: "请输入正确的电子邮件",
	    url: "请输入正确的网址",
	    date: "请输入正确的日期",
	    dateISO: "请输入正确的日期",
	    number: "请输入正确的数字",
	    digits: "只能输入整数",
	    creditcard: "请输入正确的信用卡号",
	    equalTo: "请再输一次",
	    accept: "不支持上传该附件类型",
	    filesize:"文件太大",
	    maxlength: jQuery.validator.format("长度最多为 {0} 位"),
	    minlength: jQuery.validator.format("长度最少为 {0}位"),
	    rangelength: jQuery.validator.format("长度介于 {0} 位和 {1} 位之间"),
	    range: jQuery.validator.format("大小介于 {0} 和 {1} 之间"),
	    max: jQuery.validator.format("最大为 {0} "),
	    min: jQuery.validator.format("最小为 {0} ")
	});
	
	// 中文名
	jQuery.validator.addMethod("chineseName", function(value, element) {
	    return this.optional(element) || /^[\u4E00-\u9FA5]{2,5}$/.test(value);
	}, '请输入正确的中文名');
	
	// 手机号码
	jQuery.validator.addMethod("mobileNumber", function(value, element) {
	    return this.optional(element) || /^(13[0-9]|15[0-35-9]|18[0-9]|17[06-8]|14[57])\d{8}$/.test(value);
	}, '请输入正确的手机号码');
	
	//电话的验证      
	jQuery.validator.addMethod("isPhone",function(value,element) { 
	        var length = value.length;   
	        var mobile = /^(13[0-9]|15[0-35-9]|18[0-9]|17[06-8]|14[57])\d{8}$/;   
	        var tel = /^((\(\d{2,3}\))|(\d{3}\-))?(\(0\d{2,3}\)|0\d{2,3}-)?[1-9]\d{6,7}(\-\d{1,4})?$/;   
	        return this.optional(element) || (tel.test(value) ||  (length == 11 && mobile.test(value))); 
	}, "请输入正确的联系电话");
	
	jQuery.validator.addMethod("extension", function(value, element, param) {
	    param = typeof param === "string" ? param.replace(/,/g, '|') : "png|jpe?g|gif";
	    return this.optional(element) || value.match(new RegExp("\\.(" + param + ")$", "i"));
	}, jQuery.format("请选择正确的文件格式"));
	
	//邮编 
	jQuery.validator.addMethod("postcode", function(value, element) {
	    return this.optional(element) || /^[1-9]\d{5}$/.test(value);
	}, '请输入正确的邮编');
	//身份证 
	jQuery.validator.addMethod("identitycard", function(value, element) {
	    return this.optional(element) || /^(\d{15}$|^\d{18}$|^\d{17}(\d|X|x))$/.test(value);
	}, '请输入正确的身份证号');
	//文件大小
	$.validator.addMethod('filesize', function(value, element, param) {
	    //以bytes为单位
	    return this.optional(element) || (element.files[0].size <= param) 
	});
	
	//改变错误提示的标签
	jQuery.validator.defaults.errorElement = 'span';
	```
## <a name="validate插件使用"></a>13.validate插件使用
	```js
	var caseForm = $('表单');
	if(!caseForm.length) {
            return;
    }
    caseForm.validate({
        focusCleanup:true,
        rules: {
        	//这里的title均为name属性值
            'title': {required: true},
            'field_realname[und][0][value]': {required: true},
            'field_job[und][0][value]': {required: true},
            'field_address[und][0][value]': {required: true},
            'field_phone[und][0][value]': {required: true, mobileNumber: true},
            'field_location[und][0][value]': {required: true},
            'field_company[und][0][value]': {required: true},
            'field_department[und][0][value]': {required: true},
            'field_email[und][0][value]': {required: true, email: true},
            'files[field_file_und_0]': {required: true,accept:'docx|doc|pdf|ppt|pptx',filesize:20971520}
        },
        messages: {
            'title': {
                required: '请输入标题'
            },
            'field_realname[und][0][value]': {
                required: '请输入姓名'
            },
            'field_job[und][0][value]': {
                required: '请输入职称'
            },
            'field_address[und][0][value]': {
                required: '请输入通讯地址'
            },
            'field_phone[und][0][value]': {
                required: '请输入电话或者手机号码'
            },
            'field_location[und][0][value]': {
                required: '请选择地区'
            },
            'field_company[und][0][value]': {
                required: '请选择工作单位'
            },
            'field_department[und][0][value]': {
                required: '请选择所在科室'
            },
            'field_email[und][0][value]': {
                required: '请输入电子邮箱'
            },
            'files[field_file_und_0]': {
                required: '请上传文件',
                accept:'不支持该文件类型',
                filesize:'文件太大'
            }
        },
        errorPlacement: function(error, element){
            error.appendTo(element.closest('放置错误信息的容器'));
        },
        submitHandler:function(form)
        {
            form.submit();
        }
    });
	```
## <a name="bxslider轮播图使用"></a>14.bxslider轮播图使用
	```js
	require('../lib/jquery.bxslider.js');
	
	var slider = $('放置轮播图的容器');
	if (slider.length) {
	    slider.bxSlider({
	        auto:true,
	        minSlides: 1,
	        maxSlides: 1,
	        moveSlides: 1,
	        adaptiveHeight: true
	    });
	}
	//详细API请见官网
	```
## <a name="一般选项卡"></a>15.一般选项卡
	```js
	$('.tab .title').click(function(){
    	$(this).addClass('active').siblings().removeClass('active');
    	$('.tab .content').eq($(this).index()).show().siblings().hide();
	}).eq(0).click();
	```
## <a name="新窗口打开页面"></a>16.	新窗口打开页面
	```js
	//<a href="http://zanseven007.github.io" rel="external">open link</a>
	$(document).ready(function(){
	  $("a[rel$='external']").click(function(){
	    this.target='_blank';    
	  })
	})
	```
## <a name="滚动到指定位置滑动动画"></a>17. 滚动到指定位置滑动动画
	```js
	jQuery.fn.scrollTo = function(speed){
	  var targetOffset = $(this).offset().top;
	  $(html,body).stop().animate({scrollTop: targetOffset}, speed);
	}
	//使用
	 $(#goTop).click(function(){
	  $(body).scrollTo(500);
	})
	```
## <a name="对话框控件使用"></a>18.对话框控件使用
		
	```js
	var Dialog = require('./tool/dialog.js');
	Dialog.show();
	Dialog.alert();
	Dialog.hide();
	Dialog.tip();
	//四个方法
	```

现在已经被范同学优化,详细请见[dxy-dialog](https://github.com/dxy-biz-developer/dxy-dialog)	

##	<a name="移动端PPT使用"></a>19.移动端PPT使用

	```js
	var flash = $('.放置容器 iframe, .放置容器 object');
	if (isMobile && flash.length) {
	    var js = 'http://assets.dxycdn.com/templates/core/third-party/mobile-ppt/mobile-ppt.min.js';
	    $.getScript(js, function () {
	        MobilePPT.init('biz', {
	            flash: flash,
	            dataUrlBuilder: function (nid) {
	                return 'http://www.dxy.cn/topic/biz/ppt地址/' + nid + '/ps-data.js';
	            }
	        });
	    });
	}
	```