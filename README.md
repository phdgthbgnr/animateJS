# animateJS
script pour animation background + systeme QCM/QIZ

```js

var nbecran=34;
// doc XML des reponses aux questionnaires
var reponseXml=null;
var gestionClick=new Array();

// tableau utilise pour stocker les reponses
var reponseArray=new Array();

// nombre de clic à tester
var nbclic=0;

// tableau des clics
var arrclic=Array();

// tests intemédiaires
var nbtest=1;	//nombre de tests 
var curtest=0;	// num test courant
var count=0;	// nb bonnes reponses

// anciens blo à effacer
var oldbloc=0;

// tableau des boutons cliqués du bloc info
var arrbt=Array();



// **************** FADEIN ****************

function fadein(arr)
{

	//console.log('fadein OK');
	var args=Array;
	if(!arr.length)
	{
		args = Array.prototype.slice.call(arguments);
	}else{
		args=arr;
	}

	if(args.length>0)
	{
		 //for (var i = 0; i < args.length; i++) {
      		//alert(arguments[i].speed);
			var ob=args[0];
			$(ob.id).css('display','block');
			
			if ( $.browser.msie && $.browser.version < 9 ) {
  				//$(ob.id).show();
			
				$(ob.id).delay(ob.delay).show("fast",function()
				{
					args.shift();
					fadein(args);
					if(ob.func)
					{
						ob.func.apply(null,[ob.param]);
					}
				});
					
			} else {
				var opac=1;
				if ( $.browser.msie && $.browser.version < 9 ) {
					opac=100;
				}
				
			$(ob.id).delay(ob.delay).animate({opacity:opac}, ob.speed, function() { 
			// Animation complete.
				//$.delay(ob.delay, function(){
					args.shift();
					fadein(args);
					if(ob.func)
					{
						ob.func.apply(null,[ob.param]);
					}
				//});
			});
		 }
   		}
	//}
}
// *************************************


// ************** function pour afficher et cacher des blocs ************************

function displayBlock(b,o)
{
		//var position = $(o).position();
		var p=$(o).offset();
		var y=p.top-($(b).height()/2);
		$(b).css('marginTop',y);
		if(oldbloc!=0)
		{
			$(oldbloc).hide();
		}
		$(b).show();
		oldbloc=b;
}

function displayBlock2(b,o,n)
{
		//var position = $(o).position();
		var p=$(o).offset();
		var y=p.top-($(b).height()/2);
		$(b).css('marginTop',y);
		if(oldbloc!=0)
		{
			$(oldbloc).hide();
		}
		if(oldbloc==b)
		{
			$(b).hide();
			oldbloc=0;
		}else{
			$(b).show();
			oldbloc=b;
		}
		if(isInArray(arrclic,o)==0)
		{
			arrclic.push(o);
		}
		nbclic=n;
		if(arrclic.length>=nbclic)
		{
			fadein({id:'#nav',speed:200,delay:300});
		}
		
}

function displayBlock2b(b,o,n)
{
	if($.browser.msie && $.browser.version=='8.0')
	{
		displayBlock2(b,o,n);
	}else{
		//var position = $(o).position();
		var p=$(o).offset();
		var y=p.top-($(b).height()/2);
		$(b).css('marginTop',y-50);
		if(oldbloc!=0)
		{
			$(oldbloc).hide();
		}
		if(oldbloc==b)
		{
			$(b).hide();
			oldbloc=0;
		}else{
			$(b).show();
			oldbloc=b;
		}
		if(isInArray(arrclic,o)==0)
		{
			arrclic.push(o);
		}
		nbclic=n;
		if(arrclic.length>=nbclic)
		{
			fadein({id:'#nav',speed:200,delay:300});
		}
	}
}

function displayBlock3(b,o,n)
{
		//var position = $(o).position();
		var p=$(o).offset();
		var y=p.top-($(b).height()/2);
		$(b).css('marginTop',y+60);
		if(oldbloc!=0)
		{
			$(oldbloc).hide();
		}
		if(oldbloc==b)
		{
			$(b).hide();
			oldbloc=0;
		}else{
			$(b).show();
			oldbloc=b;
		}
		if(isInArray(arrclic,o)==0)
		{
			arrclic.push(o);
		}
		nbclic=n;
		if(arrclic.length>=nbclic)
		{
			fadein({id:'#nav',speed:200,delay:300});
		}
		
}

function hideBlock(b)
{
	$(b).hide();
	oldbloc=0;
}


function fadeBlock(b)
{
	bb=$(b).attr('id');
	f='info'+bb.substr(4,bb.length);
	//index=$('#'+f+' span').index();
	//alert($(b).css("opacity"));
	if($(b).css('display')=='none')
	{
		if ( $.browser.msie && parseInt($.browser.version) < 9 ) {
			//$(b).css('opacity','100');
			//$(b).css({'filter': 'alpha(opacity=100)'});
			//$(b).animate({'filter': 'alpha(opacity=100)'},30);
			//$(b).animate({opacity:100},30);
			$('#'+f+' span').css('opacity',100);
			$(b).css('display','block');
    		//$(b).css('opacity','100');

			//$(b).children().css('display','block');
		}else{
			index=$('#'+f+' span').index();
			$('#'+f).children(index).css('opacity',1);
			$(b).css('opacity','0');
			fadein({id:b,speed:300,delay:0});
		}
	}else{
		//$(b).parent().children().find('a').css({backgroundPosition: "center 0px"});
		//console.log('b : '+b);
		//console.log('test : ' + $(b).parent().children().find('p').attr('id'));
		//popArray(arrbt,$(b).parent().children().attr('id'));
		if ( $.browser.msie && parseInt($.browser.version) < 9 ) {
			$('#'+f+' span').css('opacity',0);
			$(b).css('display','none');
    		//$(b).css('opacity','0');
			//$(b).children().css('display','block');
			/*
			$(b).animate({opacity:0},30,function(){
				$(b).css('display','none');
			});
			*/			
		}else{
			$('#'+f+' span').css('opacity',0);
			$(b).delay(0).animate({opacity:0},200,function(){
				$(b).css('display','none');
			});			
		}
	}
}


function fadeBlock_old(b)
{
	bb=$(b).attr('id');
	f='info'+bb.substr(4,bb.length);
	//index=$('#'+f+' span').index();
	//alert($(b).css("opacity"));
	if($(b).css("opacity")==0 || ($.browser.msie && parseInt($.browser.version) < 9 && $(b).css("opacity")==1))
	{
		$(b).css('display','block');
		if ( $.browser.msie && parseInt($.browser.version) <= 9 ) {
			$(b).css('opacity','100');
			//$(b).css({'filter': 'alpha(opacity=100)'});
			//$(b).animate({'filter': 'alpha(opacity=100)'},30);
			//$(b).animate({opacity:100},30);
		}else{
			//$(b).css('display','block');
			fadein({id:b,speed:300,delay:0});
		}
	}else{
		//$(b).parent().children().find('a').css({backgroundPosition: "center 0px"});
		//console.log('b : '+b);
		//console.log('test : ' + $(b).parent().children().find('p').attr('id'));
		//popArray(arrbt,$(b).parent().children().attr('id'));
		if ( $.browser.msie && parseInt($.browser.version) < 9 ) {
			$('#'+f+' span').css('opacity',0);
			$(b).animate({opacity:0},30,function(){
				$(b).css('display','none');
			});			
		}else{
			$('#'+f+' span').css('opacity',0);
			$(b).delay(0).animate({opacity:0},200,function(){
				$(b).css('display','none');
			});			
		}
	}
}



function fadeBlock2(b)
{
	//alert($(b).css("opacity"));
	if($(b).css("opacity")==0 || ($.browser.msie && $(b).css("opacity")==1))
	{
		$(b).css('display','block');
		if ( $.browser.msie && parseInt($.browser.version) <= 9 ) {
			$(b).animate({opacity:100},30);
		}else{
			fadein({id:b,speed:300,delay:0});
		}
	}else{
		if ( $.browser.msie && parseInt($.browser.version) <= 9 ) {
			$(b).animate({opacity:0},30);
			$(b).css('display','none');
			
		}else{
			$(b).delay(0).animate({opacity:0},200,function(){
				$(b).css('display','none');
			});
		}
	}
}

// ********* check le nombre de clic ***********

function ifclickedNav(p)
{
	nbclic++;
	//console.log(parseInt(p));
	if(nbclic==parseInt(p))
	{
		fadein({id:'#nav',speed:300,delay:100});
	}
}


function changeBackgroundImage(img)
{
	$('#global').css({'background-image' : 'url('+img+')'}).animate({'opacity' : '1'},500);
	/*
	$("body").animate({'opacity' : '0'},500, function() {
    	
	});
	*/
}





function moveByMarginTop(ob,i,d,s,c,v,f)
{
	// ob	: selecteur cible ('.txt')
	// i	: coordonnee finale marginTop pour animation
	// d	: delai avant de commencer l'animation
	// s	: vitesse
	// c	: selecteur cible pour effet debut animation
	// v	: effet à appliquer sur selecteur cible au debut (hide et show)
	// f	: fonction appelee à la fin
	switch(v)
	{
		case 'hide':
		$(c).css('display', 'none');
		break;
		case 'show':
		$(c).show();
		break;
	};
	var args = Array.prototype.slice.call(arguments);
	//alert(arguments.length);
	var bargs=Array;
	bargs=args.slice(7,arguments.length);
	//alert(bargs.length);
	$(ob).css('display','block');
	$(ob).delay(d).animate({marginTop:i}, s, function() {		
			//alert(bargs);
			if(typeof(f)=='function')
			{
				//alert(typeof(f));
				f.apply(null,bargs);
			}
		});
}

function hideAndFade(o,i,n,f)
{
	switch(i)
	{
		case 'hide':
		$(o).css('display', 'none');
		break;
		case 'show':
		$(o).show();
		break;
	};
	var args = Array.prototype.slice.call(arguments);
	//alert(arguments.length);
	var bargs=Array;
	arrclic.push(o);
	nbclic=n;
	if(arrclic.length>=nbclic)
	{
		fadein({id:'#nav',speed:200,delay:300});
	}
	bargs=args.slice(4,arguments.length);
	//alert(bargs.length);
	//$(ob).css('display','block');
	//$(ob).delay(d).animate({marginTop:i}, s, function() {		
			//alert(bargs);
			
			if(typeof(f)=='function')
			{
				//alert(typeof(f));
				//console.log(bargs);
				f.apply(null,bargs);
			}
}






// ******************* TEST LES REPONSES ET LES ENREGISTRE ********************

function loadXmltest(i)
{
	reponseXml=null;
	reponseArray=new Array();
	
	curtest=i;
	curtest=1;
	
	var imga = new Image();
	var img1='images/13_coeur/stitre'+i+'.png';
	var imgb = new Image();
	var img2='images/13_coeur/titre'+i+'.png';
	
	loadimg('#stitre',img1,imga);
	loadimg('#titre',img2,imgb);
	
	/*
	$('.conclu').css('display','none');
	$('.conclu p').html('');
	*/
	$('.nodisplay').css('display','none');

	for (var t=1;t<6;t++)
	{

		$('.rep'+t+' a').css({ backgroundPosition: '0px 0px'});

	}

	$.ajax({
		type:'GET', 
		dataType:'text/xml', 
		async: true, 
		url:"xml/reponses"+i+".xml?123545454",
		
		success:function(dt){ parseXML(dt,true)},
		error: function(msg){
			console.log('erreur xml 1'+msg.toString())
			}
		});
}


function loadXmltest2(i)
{
	reponseXml=null;
	reponseArray=new Array();

	curtest=i;
	
	$('#titre').css('backgroundPosition','0px '+((i-1)*-23)+'px');
	/*
	$('.conclu').css('display','none');
	$('.conclu p').html('');
	*/
	$('.nodisplay').css('display','none');

	for (var t=1;t<5;t++)
	{

		$('.rep'+t+' a').css({ backgroundPosition: '0px 0px'});

	}

	$.ajax({type:'GET', dataType:'text/xml', async: true, url:"xml/reponsesb"+i+".xml?123545454",
		success:function(dt){ parseXML(dt,false)},
		error: function(msg){alert('erreur xml '+msg)}
	});
}

function loadimg(cont,image,img){
	$(img).load(function () {    
    	if ( $.browser.msie && $.browser.version < 9 ) {
		//
		}else{
			$(this).hide();
		}
		if($(cont).children().size()>0){
			$(cont).children().remove();
		}
     	$(cont).append(this);
      	if ( $.browser.msie && $.browser.version < 9 ) {
			//
		}else{
			$(this).fadeIn();
		}
	}).error(function () {
   	}).attr('src', image);
}


function checkReponse(t)
{
	
	getReponse(t);

}



function getReponse(t)
{
	//console.log(reponseXml);
	//console.log($(reponseXml).find("rep").attr("id")==t);
	var leafNodes = $(reponseXml).find('resp').filter(function ()
    {
        //return $(this).children().length === 0;
		//console.log($(this).attr('rep'));
		return $(this).attr('rep') === '1';
    });
    

    count = leafNodes.length;
	//console.log(count);
	$(reponseXml).find('resp').each(
		function()
		{

			//console.log($(this));
			//console.log($(this).attr('id'));
			if($(this).attr('id')==t)
			{
				//console.log($(this).attr('rep'));
				//return($(this).attr('rep'));
				var r=$(this).attr('rep');
				//var cl=$(this).attr('conclu');
				if(r=='1')
				{
					//console.log(count);
					//$('.'+t+' a').addClass("good");
					$('.'+t+' a').css({ backgroundPosition: '0 -84px'});
					/*
					$('.conclu p').html(cl);
					$('.conclu').css('display','block');
					*/
					//console.log(nbtest+' '+curtest);
					var tmp=$.parseJSON('{"'+t+'":"'+r+'"}');
					reponseArray.push(tmp);
					if(reponseArray.length>=count)
					{
						//$('.valider').hide();
						$('.nodisplay').css('display','block');
						$('.nodisplay').show();
					}
					/*
					console.log(nbtest);
					console.log(curtest);
					console.log(reponseArray.length);
					console.log(count);
					*/
					if(nbtest==curtest && reponseArray.length>=count)
					{
						//console.log('ok');
						//$('.nodisplay').css('display','none');
						$('.suite').css('display','block');
						$('.suite').show();
					/*}else{
						$('.nodisplay').css('display','block');
						$('.nodisplay').show();	
					*/
					}
			
					//console.log('.'+t+' a');
				}else{
					$('.'+t+' a').css({ backgroundPosition: '0 -168px'});
					//$('.'+t+' a').addClass("bad");
				}
				//var tmp=$.parseJSON('{"'+t+'":"'+r+'"}');
				//console.log(t);
				
				//console.log(t);
				//console.log(reponseArray);

			}
		}
	);
}



function parseXML(dt,b)
{
	// parsing xml
	if ( $.browser.msie && $.browser.version < 9 ) {
		reponseXml=new ActiveXObject("Microsoft.XMLDOM");
  		reponseXml.async="false";
  		reponseXml.loadXML(dt);
		if(b) affquestions();
		if(!b) affquestion();
	}else{
		parser = new DOMParser();
		reponseXml = parser.parseFromString(dt , "text/xml");
		if(b) affquestions();
		if(!b) affquestion();
	}

	//console.log('xml '+dt);
	//getReponse(t);
}


function affquestions()
{
		
	$(reponseXml).find('resp').each(
		function()
		{
			var t=$(this).attr('texte');
			var i=$(this).attr('id');
			var tt=$(this).text();
			$('.'+i+' span').empty().append(tt);
		});

}

function affquestion()
{
		
	//console.log($(reponseXml).children(0).text());
	$('.txt3').empty().append($(reponseXml).children(0).text());
}










function validQuiz()
{
	for (var t=1;t<15;t++)
	{

		$('.rep'+t+' a').css({ backgroundPosition: '0 -84px'});

	}
	$('.valider').hide();
	$('.nodisplay').css('display','block');
	$('.nodisplay').show();
	
	$('.conclu').css('display','block');
	fadeBlock('.conclu');
	
}

function isInArray(a,i)
{
	if(a.length==0) return 0;
	for(var t in a)
	{
		//console.log('array : '+a[t]);
		if (a[t]==i) return 1;
	}
	return 0;
}

function isInArrayInd(a,i)
{
	if(a.length==0) return -1;
	for(var t in a)
	{
		//console.log('array : '+a[t]);
		if (a[t]==i) return t;
	}
	return -1;
}

function isInArrayProp(a,i)
{
	if(a.length==0) return 0;
	for(var t in a)
	{
		//console.log('array : '+a[t][i]);
		if (a[t][i]!=undefined) return 1;
	}
	return 0;
}

// ***************************************************************************



// gestion clic sur bouton plus

var cb1=0;
var cb2=0;

function plus(b,n){
	nbclic=n;
	cb1=b;
	//console.log(nbclic);
	if(cb2!=0)
	{
		fadeBlock('#'+cb2);
		//return false;
	}
	if(cb1==cb2)
	{
		fadeBlock('#'+cb1);
		cb1=0;
		cb2=0;
	}
	
	cb2=cb1;
	
	if(isInArray(arrclic,cb1)==0 && cb1!=0)
	{
		arrclic.push(cb1);
	}
	/*	
	if(arrclic.length==0)
	{
		arrclic.push(cb1);
	}
	*/
	fadeBlock('#'+cb1);
	
	if(arrclic.length>=nbclic)
	{
		if(arguments.length>2)
		{
			if ( $.browser.msie && $.browser.version < 9 ) {
				$(arguments[2]).animate({opacity:100},30);
			}else{
				fadein({id:arguments[2],speed:200,delay:300});	
			}
		}
		fadein({id:'#nav',speed:200,delay:300});
	}
	return false;
}


function plus2(b,n){
	nbclic=n;
	cb1=b;
	if(cb2!=0)
	{
		fadeBlock2('#'+cb2);
		//return false;
	}
	if(cb1==cb2)
	{
		fadeBlock2('#'+cb1);
		cb1=0;
		cb2=0;
	}
	
	cb2=cb1;
	
	if(isInArray(arrclic,cb1)==0 && cb1!=0)
	{
		arrclic.push(cb1);
	}
	/*	
	if(arrclic.length==0)
	{
		arrclic.push(cb1);
	}
	*/
	fadeBlock2('#'+cb1);
	
	if(arrclic.length>=nbclic)
	{
		if(arguments.length>2)
		{
			if ( $.browser.msie && $.browser.version < 9 ) {
				$(arguments[2]).animate({opacity:100},30);
			}else{
				fadein({id:arguments[2],speed:200,delay:300});	
			}
		}
		fadein({id:'#nav',speed:200,delay:300});
	}
	return false;
}

function plusplus(b,n,pcent){ // pcent = pourcentage de decalage du popup par au bouton
	nbclic=n;
	//cb1=b;
	cb1='bloc'+b.substr(4,b.length);
	
	index=$('#'+b+' span').index();
	/*
	// affichage fleche
	if ( $.browser.msie && $.browser.version < 9 ) {
		$('#'+b+' span').css('opacity',100);
		//$('#'+b).children(index).css('opacity',1);
		//$('#'+b).children(index).css({'filter': 'alpha(opacity=1)'});
	}else{
		$('#'+b).children(index).css('opacity',1);	
	}
	*/
			
	var hb=$('#'+cb1).outerHeight();
	var wb=$('#'+cb1).outerWidth();
	var pos=$('#'+b).position();
	var y=pos.top;
	var x=pos.left;
	var x2=x+$('#'+b).width()/2;
	var xb2=x2-wb/2;
	var h=$('#'+b).height();
	var py;
	var decal=((pcent*wb)/100)/2;
	xb2+=decal;
	$('#'+cb1).css('left',xb2+'px');
	
	switch(index)
	{
		case 0:
		py=y-hb;
		$('#'+cb1).css('top',py+'px');
		break;
		case 1:
		py=y+h;
		$('#'+cb1).css('top',py+'px');
		break
	}
	
	//console.log(nbclic);
	if(cb2!=0 && cb2!=cb1)
	{
		fadeBlock('#'+cb2);
		//return false;
	}
	if(cb1==cb2 && cb1!=0 && cb2!=0)
	{
		fadeBlock('#'+cb1);
		cb1=0;
		cb2=0;
	}
	
	cb2=cb1;
	
	if(isInArray(arrclic,cb1)==0 && cb1!=0)
	{
		arrclic.push(cb1);
	}
	/*	
	if(arrclic.length==0)
	{
		arrclic.push(cb1);
	}
	*/
	if(cb1!=0) fadeBlock('#'+cb1);
	
	if(arrclic.length>=nbclic)
	{
		if(arguments.length>3)
		{
			if ( $.browser.msie && $.browser.version < 9 ) {
				$(arguments[3]).css('display','block');
				$(arguments[3]).animate({'filter': 'alpha(opacity=100)'},30);
			}else{
				fadein({id:arguments[3],speed:200,delay:300});	
			}
		}
		fadein({id:'#nav',speed:200,delay:300});
	}
	return false;
}


function plus3(b,n){
	nbclic=n;
	cb1=b;
	if(cb2!=0)
	{
		//return false;
	}
	if(cb1==cb2)
	{
		cb1=0;
		cb2=0;
	}
	
	cb2=cb1;
	
	if(isInArray(arrclic,cb1)==0 && cb1!=0)
	{
		arrclic.push(cb1);
	}
	/*	
	if(arrclic.length==0)
	{
		arrclic.push(cb1);
	}
	*/
	
	if(arrclic.length>=nbclic)
	{
		if(arguments.length>2)
		{
			if ( $.browser.msie && $.browser.version < 9 ) {
				$(arguments[2]).animate({opacity:100},30);
			}else{
				fadein({id:arguments[2],speed:200,delay:300});	
			}
		}
		fadein({id:'#nav',speed:200,delay:300});
	}
	return false;
}


function clicked(t)
{
	//console.log($(t).parent().attr("id"));
	var n=$(t).parent().attr("id")
	if(isInArray(arrbt,n)==0)
	{
		arrbt.push(n);
		$(t).css({backgroundPosition: "center -94px"});
	}else{
		popArray(arrbt,n);
		$(t).css({backgroundPosition: "center 0px"});
	}
	
}

function popArray(a,i)
{
	for(var t in a)
	{
		//console.log('array : '+a[t][i]);
		if (a[t]==i) a.pop(t);
	}
}


function animBackground()
{
	
	var py;
	var px;
	var pos;
	var y;
	var obj;
	var obt;
	this.obt=this;
	
	this.animation=function(ob,x,y)
	{
	this.py=y*60;
	this.px=x;
	this.pos=this.px+"px -"+this.py+"px";
	this.y=y+1;
	this.obj=ob;
	
	this.fin=function()
	{
		if(this.y<5)
		{
			this.animation(this.obj,this.px,this.y);
		}else{
			this.animation(this.obj,this.px,0);
		}
	}
	var t=this.obt;
	var ppx=this.px;
	var objj=this.obj;
	var yy=this.y;
	$(this.obj).animate({"background-position" : this.pos},100,function(){
		
		if(yy<5)
		{
			t.animation(objj,ppx,yy);	
		}else{
			t.animation(objj,ppx,0);
		}
		});
	
	}
	
}


function clickButton(ob)
{
	$(ob).clearQueue();
  	$(ob).stop();
	var a=isInArrayGetProp(gestionClick,ob);
	if(gestionClick[a][2]==0)
	{
		gestionClick[a][2]=1;
		$(ob).css('backgroundPosition', "-120px 0");
		gestionClick[a][1].animation(ob,-120,0);
	}else{
		gestionClick[a][2]=0;
		$(ob).css('backgroundPosition', "0px 0");
		gestionClick[a][1].animation(ob,0,0);
	}
	
	isInArraySetdiffProp(gestionClick,ob);
}

function clickButtonload(ob,img1,img2)
{
	$(ob).clearQueue();
  	$(ob).stop();
	var a=isInArrayGetProp(gestionClick,ob);
	if(gestionClick[a][2]==0)
	{
		gestionClick[a][2]=1;
		$(ob).css('backgroundPosition', "-120px 0");
		gestionClick[a][1].animation(ob,-120,0);
		var img = new Image();
 			$(img).load(function () {    
      			if ( $.browser.msie && $.browser.version < 9 ) {
					//
				}else{
					$(this).hide();
				}
				if($('#img1').children().size()>0){
					$('#img1').children().remove();
				}
     			$('#img1').append(this);
				if ( $.browser.msie && $.browser.version < 9 ) {
					//
				}else{
					$(this).fadeIn();
				}
      			
				affinfo(cur);
    		}).error(function () {
   			}).attr('src', img2);
	}else{
		gestionClick[a][2]=0;
		$(ob).css('backgroundPosition', "0px 0");
		gestionClick[a][1].animation(ob,0,0);
		
		var img = new Image();
 			$(img).load(function () {    
      			if ( $.browser.msie && $.browser.version < 9 ) {
					//
				}else{
					$(this).hide();
				}
				if($('#img1').children().size()>0){
					$('#img1').children().remove();
				}
     			$('#img1').append(this);
      			if ( $.browser.msie && $.browser.version < 9 ) {
					//
				}else{
					$(this).fadeIn();
				}
				affinfo(cur);
    		}).error(function () {
   			}).attr('src', img1);
	}
	
	isInArraySetdiffProp(gestionClick,ob);

}


function hoverButton(ob){
	
	var a=isInArrayGetProp(gestionClick,ob);
	if(gestionClick[a][2]==0)
	{
		$(ob).clearQueue();
  		$(ob).stop();
		$(ob).css('backgroundPosition', "-60px 0");
		gestionClick[a][1].animation(ob,-60,0);
	}
}

function hoverOut(ob)
{
	var a=isInArrayGetProp(gestionClick,ob);
	if(gestionClick[a][2]==0)
	{
		$(ob).clearQueue();
  		$(ob).stop();
		$(ob).css('backgroundPosition', "0 0");
		gestionClick[a][1].animation(ob,0,0);
	}
}


function isInArrayGetProp(a,i){
	if(a.length==0) return -1;
	for(var t in a)
	{
		//console.log('array : '+a[t][i]);
		if (a[t][0]==i) return t;
	}
	return -1;
}

function isInArraySetdiffProp(a,i){
	if(a.length==0) return 0;
	for(var t in a)
	{
		if (a[t][0]!=i)
		{
			$(a[t][0]).clearQueue();
  			$(a[t][0]).stop();
			$(a[t][0]).css('backgroundPosition', "0 0");
			a[t][2]=0;
			a[t][1].animation(a[t][0],0,0);
		}
	}
	return 0
}

function createArray(i){
	var a=new Array();
	a.push(i);
	a.push(new animBackground());
	a[1].animation(i,0,0);
	a.push(0);
	gestionClick.push(a);
}

function shuffle(a){
   var j = 0;
   var valI = '';
   var valJ = valI;
   var l = a.length - 1;
   while(l > -1)
   {
		j = Math.floor(Math.random() * l);
		valI = a[l];
		valJ = a[j];
		a[l] = valJ;
		a[j] = valI;
		l = l - 1;
	}
	return a;
 }
 ```
