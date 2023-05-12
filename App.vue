<script>
	import { findConfig } from "./api/index";
export default {
  onLaunch: function () {
    console.log("App Launch");
	this.checkkefu()
  },
  onShow: function () {
    console.log("App Show");
  },
  onHide: function () {
    console.log("App Hide");
  },
  methods:{
	  checkkefu(){
		  findConfig().then((res)=>{
		  	// console.log(res.result)
			(function(a, b, c, d, e, j, s) {
			    a[d] = a[d] || function() {
			        (a[d].a = a[d].a || []).push(arguments)
			    };
			    j = b.createElement(c),
			        s = b.getElementsByTagName(c)[0];
			    j.async = true;
			    j.charset = 'UTF-8';
			    j.src = 'https://static.meiqia.com/widget/loader.js';
			    s.parentNode.insertBefore(j, s);
			})(window, document, 'script', '_MEIQIA');
			_MEIQIA('entId', res.result.meiqia);
			
		  })
		      
		
	  }
  }
  
};
</script>

<style  lang="scss">
/*每个页面公共css */
@import "colorui/main.css";
@import "colorui/icon.css";
@import "style/FontStyle.css";
@import "@/uni_modules/uview-ui/index.scss";
@import "@/components/iconfont/iconfont.css";
body {
  -webkit-text-size-adjust: none;
}
/* 超出一行省略号 */
.one-omit {
  white-space: nowrap; /*规定段落中的文本不进行换行*/
  overflow: hidden; /*内容会被修剪，并且其余内容是不可见的。*/
  text-overflow: ellipsis; /*显示省略号来代表被修剪的文本*/
}

.two-omit {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  overflow: hidden;
  text-overflow: ellipsis;
  -webkit-box-orient: vertical;
}
img {
  width: 100%;
}
</style>
