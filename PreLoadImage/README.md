# ͼƬԤ����
> ʵ����ͼƬԤ���ص�**�����**

���ǰԤ����ͼƬ��ʹ��jquery��װ�����һ��������ʵ������չʾ��

* ��ҳ��ͼƬ������أ�Ԥ�����ðٷֱ���ʾ����
* qq��������Ԥ���أ�Ԥ������ʾloading��Ϣ
* ��ҳ��������Ԥ����

## ͼƬԤ���ز����Ҫ����
### ���캯���ͳ�ʼ���ֶ�
`
//���캯��
function PreLoad(imgs, options){
	this.imgs = (typeof imgs === "string") ? [imgs] : imgs;
	this.ops = $.extend({}, PreLoad.DEFAULTS, options);	//����options����defaultֵ

	this.ops.order == 'oredered' ? this._oredered() : this._unoredered();	//�жϵ��������������
}
//��ʼ���ֶ�
PreLoad.DEFAULTS = {
	order: 'unoredered',	//Ĭ���������
	each: null,	//ÿ��ͼƬ������Ϻ�ִ��
	all: null	//����ͼƬ������Ϻ�ִ��
};
`
### �������
`
PreLoad.prototype._unoredered = function() {	//�������
	var imgs = this.imgs,
		ops = this.ops,
		count = 0,
		len = imgs.length;

	$.each(imgs, function(i, src){
		if((typeof src) != "string") return;

		var oImage = new Image();

		$(oImage).on('load error', function(){
			count++;

			ops.each && ops.each(count);

			if(count >= len){
				ops.all && ops.all();
			}
		})

		oImage.src = src;
	})
}
`

### �������
`
PreLoad.prototype._oredered = function() {	//�������
	var imgs = this.imgs,
		ops = this.ops,
		count = 0,
		len = imgs.length;

	load();

	function load() {
		var oImage = new Image();

		$(oImage).on("load error", function () {
			ops.each && ops.each(count);
			count++;

			if(count >= len){
				//ִ�����
				ops.all && ops.all();
			}else{
				load();
			}
		})

		oImage.src = imgs[count];
	}
}
`

### ��չ����
`
$.extend({
	preload: function(imgs, options){
		return new PreLoad(imgs, options);
	}
})
`

### ��չ��������
`
$.preload(ImageUrl Array or ImageUrl, {
	order: '',
	each: function(count){
	},
	all: function(){
	}
});
`

## ����ѧϰ
��js���������µ�ѧϰ����Ҫ������֪ʶ�㣺
1. (function($){...})(jQuery) ʵ���˱հ���ʹ����ı����������������js���Ӱ�졣
2. ͨ��Math��Max��Min������������ж�С��0�ʹ���ͼƬ������
3. Image�����onload�¼�Ҫд��src��ֵǰ�棬�����ͼƬ���棬�����޷�ִ��onload�¼���
3. ��js�����������һ�����˽⡣
4. jquery����������������ȡ������
