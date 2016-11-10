#通用表单项校验规范

##验证手机号
	验证规则：以1开头11位数字。

	```javascript
	var  re = /^1\d{10}$/;
	re.test(str) 
	```

##验证电话/传真号码
	验证规则：区号+号码，区号以0开头，3位或4位
	号码由7位或8位数字组成，区号与号码之间可以无连接符，也可以“-”连接

	```javascript
	re = /^0\d{2,3}-?\d{7,8}$/;
	re.test(str)
	```

##验证邮箱
	验证规则：姑且把邮箱地址分成“第一部分@第二部分”这样
	第一部分：由字母、数字、下划线、短线“-”、点号“.”组成，
	第二部分：为一个域名，域名由字母、数字、短线“-”、域名后缀组成，
	而域名后缀一般为.xxx或.xxx.xx，一区的域名后缀一般为2-4位，如cn,com,net，现在域名有的也会大于4位

  ```javascript
	var re = /^(\w-*\.*)+@(\w-?)+(\.\w{2,})+$/;
	re.test(str)
	```

##验证身份证号码
	身份证号分为两种，旧的为15位，新的为18位。

	身份证15位编码规则：dddddd yymmdd xx p    
	其中 dddddd：地区码; yymmdd: 出生年月日; xx: 顺序类编码，无法确定; p: 性别，奇数为男，偶数为女 
	
	身份证18位编码规则：dddddd yyyymmdd xxx y    
	其中 dddddd：地区码; yyyymmdd: 出生年月日; xxx:顺序类编码，无法确定，奇数为男，偶数为女; y: 校验码
	校验码数值可通过前17位计算获得，计算的公式见程序
	一些需要用到的常数：
		18位号码加权因子为(从右到左) Wi = [ 7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2,1 ]
		验证位 Y = [ 1, 0, 10, 9, 8, 7, 6, 5, 4, 3, 2 ]
	校验位计算公式：
		Y_P = mod( ∑(Ai×Wi),11 )
		i为身份证号码从右往左数的 2...18 位; 
		Y_P为脚丫校验码所在校验码数组位置。

  ```javascript
	function checkIdcard(num){
	    num = num.toUpperCase();
	    //身份证号码为15位或者18位，15位时全为数字，18位前17位为数字，最后一位是校验位，可能为数字或字符X。
	    if (!(/(^\d{15}$)|(^\d{17}([0-9]|X)$)/.test(num)))
	    {
	        alert('输入的身份证号长度不对，或者号码不符合规定！\n15位号码应全为数字，18位号码末位可以为数字或X。');
	        return false;
	    }
	    //校验位按照ISO 7064:1983.MOD 11-2的规定生成，X可以认为是数字10。
	    //下面分别分析出生日期和校验位
	    var len, re;
	    len = num.length;
	    if (len == 15)
	    {
	        re = new RegExp(/^(\d{6})(\d{2})(\d{2})(\d{2})(\d{3})$/);
	        var arrSplit = num.match(re);
	 
	        //检查生日日期是否正确
	        var dtmBirth = new Date('19' + arrSplit[2] + '/' + arrSplit[3] + '/' + arrSplit[4]);
	        var bGoodDay;
	        bGoodDay = (dtmBirth.getYear() == Number(arrSplit[2])) && 
						((dtmBirth.getMonth() + 1) == Number(arrSplit[3])) && 
						(dtmBirth.getDate() == Number(arrSplit[4]));
	        if (!bGoodDay)
	        {
	            alert('输入的身份证号里出生日期不对！');
	            return false;
	        }
	        else
	        {
	                //将15位身份证转成18位
	                //校验位按照ISO 7064:1983.MOD 11-2的规定生成，X可以认为是数字10。
	                var arrInt = new Array(7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2);
	                var arrCh = new Array('1', '0', 'X', '9', '8', '7', '6', '5', '4', '3', '2');
	                var nTemp = 0, i;
	                num = num.substr(0, 6) + '19' + num.substr(6, num.length - 6);
	                for(i = 0; i < 17; i ++)
	                {
	                    nTemp += num.substr(i, 1) * arrInt[i];
	                }
	                num += arrCh[nTemp % 11];
	                return true;
	        }
	    }
	    if (len == 18)
	    {
	        re = new RegExp(/^(\d{6})(\d{4})(\d{2})(\d{2})(\d{3})([0-9]|X)$/);
	        var arrSplit = num.match(re);
	 
	        //检查生日日期是否正确
	        var dtmBirth = new Date(arrSplit[2] + "/" + arrSplit[3] + "/" + arrSplit[4]);
	        var bGoodDay;
	        bGoodDay = (dtmBirth.getFullYear() == Number(arrSplit[2])) && 
						((dtmBirth.getMonth() + 1) == Number(arrSplit[3])) && 
						(dtmBirth.getDate() == Number(arrSplit[4]));
	        if (!bGoodDay)
	        {
	            alert('输入的身份证号里出生日期不对！');
	            return false;
	        }
		    else
		    {
		        //检验18位身份证的校验码是否正确。
		        //校验位按照ISO 7064:1983.MOD 11-2的规定生成，X可以认为是数字10。
		        var valnum;
		        var arrInt = new Array(7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2);
		        var arrCh = new Array('1', '0', 'X', '9', '8', '7', '6', '5', '4', '3', '2');
		        var nTemp = 0, i;
		        for(i = 0; i < 17; i ++)
		        {
		            nTemp += num.substr(i, 1) * arrInt[i];
		        }
		        valnum = arrCh[nTemp % 11];
		        if (valnum != num.substr(17, 1))
		        {
		            alert('18位身份证的校验码不正确！应该为：' + valnum);
		            return false;
		        }
		        return true;
		    }
	    }
		return false;
	}
	```
##验证邮编
	验证规则：非0开头的6位数字

  ```javascript
	var re= /^[1-9][0-9]{5}$/
	re.test(str)
	```

##验证ipv4
	验证规则：分为4段，十进制数字表示，每段数字范围为0~255，段与段之间用英文句点“.”隔开
	
	```javascript
	var re = /^(25[0-5]|2[0-4][0-9]|[0-1]{1}[0-9]{2}|[1-9]{1}[0-9]{1}|[1-9])\.(25[0-5]|2[0-4][0-9]|[0-1]{1}[0-9]{2}|[1-9]{1}[0-9]{1}|[1-9]|0)\.(25[0-5]|2[0-4][0-9]|[0-1]{1}[0-9]{2}|[1-9]{1}[0-9]{1}|[1-9]|0)\.(25[0-5]|2[0-4][0-9]|[0-1]{1}[0-9]{2}|[1-9]{1}[0-9]{1}|[0-9])$/;
	re.test(str)
	```
