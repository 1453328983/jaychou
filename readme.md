#python爬取QQ音乐某个歌手的全部专辑歌词并生成文字云图片
> 背景:最近因为看到了一个分析民谣歌手唱了什么而印发的思考,准备慢慢的也做一个类似的分析,实践一下自己的想法.

##程序可以做什么
- 获取某个歌手的QQ音乐的专辑歌曲
- 生成歌词云照片
效果图
![image](https://github.com/fantasysea/jaychou/raw/master/pics/songfile.png)
![image](https://github.com/fantasysea/jaychou/raw/master/pics/jaychou.jpg)

##工作原理
- 首先我们看看qq音乐专辑的url是什么.以周杰伦为例 `https://y.qq.com/n/yqq/singer/0025NhlN2yWrP4.html#tab=album&`
	![image](https://github.com/fantasysea/jaychou/raw/master/pics/albumurl.png)
	1. 首先我们按照图片的顺序找到我们需要的真正的url,可以搜素某个专辑名字,看看哪里返回来了.然后我们找到了专辑信息的真正url是:
	`https://c.y.qq.com/v8/fcg-bin/fcg_v8_singer_album.fcg?g_tk=5381&jsonpCallback=MusicJsonCallbacksinger_album&loginUin=0&hostUin=0&format=jsonp&inCharset=utf8&outCharset=utf-8&notice=0&platform=yqq&needNewCode=0&order=time&begin=0&num=300&exstatus=1&singermid=0025NhlN2yWrP4'`
	我们分析一下这个url,发现里面有几个参数很可疑
	> &singermid=0025NhlN2yWrP4
	> begin=0&num=30
	经过测试我发现singermid就是歌手专辑主页上的最后一个字段0025NhlN2yWrP4
	begin=0&num=30 就是歌手专辑分页使用的起点和终点,那么我们平时在做抓取的时候可以不分页行不行,把num加大就可以了吧,经过测试,发现确实是可以的,那么我直接把num设置成300,这样基本不会有歌手有这么大的专辑数量.
	2. 然后我们就看看这个url返回了的专辑数据是什么.
``` 
MusicJsonCallbacksinger_album({"code":0,"data":{"list":[{"Fattribute_5":"0","Ftype":"0","albumID":"1458791","albumMID":"003RMaRI1iFoYd","albumName":"周杰伦的床边故事","albumtype":"录音室专辑","company":"杰威尔音乐有限公司","desc":"【周杰伦全新数字专辑《周杰伦的床边故事》震撼上线！\n预购用户以及当前购买用户可收听、下载专辑内全部歌曲。】\n\n夜深了\n猫头鹰出没\n数完第一千零一个尖...","lan":"国语","listen_count":"243514445","pubTime":"2016-06-24","score":"950","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"1","albumID":"1366240","albumMID":"004c8Md02WHHju","albumName":"周杰伦魔天伦世界巡回演唱会","albumtype":"现场专辑","company":"杰威尔音乐有限公司","desc":"从2013年5月17日首场，到2015年12月20日，「魔天伦演唱会」巡回全世界共76场，现场观赏人次超过180万以上，这76个独特的夜晚，累积超越...","lan":"国语","listen_count":"5234745","pubTime":"2016-05-10","score":"949","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"10","albumID":"1306793","albumMID":"001uJFiE0tbGGa","albumName":"英雄","albumtype":"EP单曲","company":"杰威尔音乐有限公司","desc":"等待《英雄》终于现声！\n周杰伦 2016最新创作单曲《英雄》\n属于召唤师们的战歌\n号召各界英雄  击响战鼓  全面出动！\n\n人生不是一个人的游戏\n一...","lan":"国语","listen_count":"53918399","pubTime":"2016-03-24","score":"903","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"1","Ftype":"10","albumID":"1268858","albumMID":"001V8fw21OdEnP","albumName":"Try","albumtype":"EP单曲","company":"杰威尔音乐有限公司","desc":"周杰伦首次操刀监制动画电影主题曲 \n推荐徒弟派伟俊写曲演唱\n\n《功夫熊猫3》继前作的剧情，故事讲述神龙大侠熊猫「阿宝」(台湾译阿波)在寻找自己身世之...","lan":"英语","listen_count":"24276996","pubTime":"2016-01-06","score":"929","shoufa":1,"singerID":"1043869","singerMID":"000kZUyu0aPr9t","singerName":"派伟俊","singers":[{"singer_id":"1043869","singer_mid":"000kZUyu0aPr9t","singer_name":"派伟俊"},{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"0","albumID":"852856","albumMID":"001uqejs3d6EID","albumName":"哎呦，不错哦","albumtype":"录音室专辑","company":"杰威尔音乐有限公司","desc":"年度特大号期待！\n很Jay的一张专辑\n周杰伦 Jay Chou 哎呦，不错哦\nJAY语录里最被流传模仿的一句  最周杰伦的一张创作\n\n以幽默拉开序幕...","lan":"国语","listen_count":"286011254","pubTime":"2014-12-26","score":"962","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"1","Ftype":"6","albumID":"453899","albumMID":"003b9EXT2QU0T7","albumName":"黄俊郎的黑","albumtype":"参与过的专辑","company":"杰威尔音乐有限公司","desc":"这是一个你很少见过的、周杰伦的样子\n \n一个用他一生无法离开的钢琴，来诠释一本美丽的书、一段美丽旅程的周杰伦\n \n阿郎老师说：「写出这首钢琴曲的杰伦...","lan":"纯音乐","listen_count":"2083764","pubTime":"2013-12-25","score":"937","shoufa":1,"singerID":"13886","singerMID":"002Z462D2amcz9","singerName":"杨瑞代","singers":[{"singer_id":"13886","singer_mid":"002Z462D2amcz9","singer_name":"杨瑞代"},{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"0","albumID":"194021","albumMID":"003Ow85E3pnoqi","albumName":"十二新作","albumtype":"录音室专辑","company":"杰威尔音乐有限公司","desc":"当时光敲第12响时，是该听周杰伦的时候了…\n周杰伦2012最新专辑 「十二新作」\n2012年12月 28日 压轴登场\n\nJay.12的巧合与奥妙\n出...","lan":"国语","listen_count":"289819592","pubTime":"2012-12-28","score":"939","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"0","albumID":"85650","albumMID":"003KNcyk0t3mwg","albumName":"惊叹号","albumtype":"录音室专辑","company":"杰威尔音乐有限公司","desc":"靠自己创作跑道才是王道！\n2011周杰伦第11号作品【惊叹号】\n炫技玩乐出神入化满载励志、传承、欢乐、疗伤。\n用他的音乐获得力量与幽默向未来的逆境热...","lan":"国语","listen_count":"123594626","pubTime":"2011-11-11","score":"878","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"1","albumID":"67340","albumMID":"002o2a5G46ETLr","albumName":"The Era 2010 超时代演唱会","albumtype":"现场专辑","company":"杰威尔音乐有限公司","desc":"目眩神迷、惊叹震撼、感动沉醉\n2000～2010 十年精华一夜经典\n\n周杰伦 超时代 演唱会\nJay Chou The Era World Tour","lan":"国语","listen_count":"31261895","pubTime":"2011-01-25","score":"871","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"0","albumID":"56705","albumMID":"000bviBl4FjTpO","albumName":"跨时代","albumtype":"录音室专辑","company":"杰威尔音乐有限公司","desc":"跨越自己 创新时代\n\n周杰伦 2010第十辑【跨时代】\n\n跨乐古今．飞越想象．创意绝伦．超时感动\n\n华语乐坛天王周杰伦每次带给大家的全新音乐每每引领...","lan":"国语","listen_count":"166349245","pubTime":"2010-05-18","score":"837","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"0","albumID":"36062","albumMID":"002Neh8l0uciQZ","albumName":"魔杰座","albumtype":"录音室专辑","company":"杰威尔音乐有限公司","desc":"周杰伦2008年最新魔幻杰作“魔杰座”魔术师与小丑的顶尖对决 周杰伦粉墨登场秀时尚版“烟熏小丑妆”9/24预购起跑 12款“魔杰桌历”迎2009 “...","lan":"国语","listen_count":"271573125","pubTime":"2008-10-15","score":"787","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"1","albumID":"33802","albumMID":"004cPHRr3nkmJe","albumName":"2007世界巡回演唱会","albumtype":"现场专辑","company":"杰威尔音乐有限公司","desc":"睽违三年多，《周杰伦2007世界巡回演唱会》首站于11月10日在台北县立板桥体育场揭开序幕，之後穿梭香港、马来西亚、日本、新加坡、上海、广州等地！在...","lan":"国语","listen_count":"22521336","pubTime":"2008-01-30","score":"756","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"11","albumID":"33749","albumMID":"004RBqID0tABKW","albumName":"大灌篮 电影原声带","albumtype":"EP单曲","company":"杰威尔音乐有限公司","desc":"少年世杰(周杰伦 饰)从小在功夫学校长大，练就了一身好功夫并拥有异于常人的手感，无意间展现“中投”的神技，被街头讨生活且鬼点子特多的立叔(曾志伟 饰...","lan":"国语","listen_count":"18961284","pubTime":"2008-01-18","score":"741","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"0","albumID":"33021","albumMID":"002eFUFm2XYZ7z","albumName":"我很忙","albumtype":"录音室专辑","company":"杰威尔音乐有限公司","desc":"再忙 … 也要陪你听一首歌！ 周杰伦 2007全新专辑「我很忙」！\n＊忙电影宣传 忙拍广告 忙新专辑 忙演唱会\n周杰伦的第8张专辑「我很忙」(JAY...","lan":"国语","listen_count":"304623451","pubTime":"2007-11-02","score":"797","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"11","albumID":"60745","albumMID":"002pPeJ54P94c0","albumName":"黄金甲","albumtype":"EP单曲","company":"杰威尔音乐有限公司","desc":"热爱中国功夫的周杰伦，在张艺谋电影《满城尽带黄金甲》的演出，首次古装造型饰演的二王子─韩元杰，是个武功高强、征战沙场的大将军。之前片中的主题曲“菊花...","lan":"国语","listen_count":"7546075","pubTime":"2006-12-11","score":"848","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"0","albumID":"13004","albumMID":"002jLGWe16Tf1H","albumName":"依然范特西","albumtype":"录音室专辑","company":"阿尔发音乐","desc":"一年推出一张专辑的周杰伦，果然一出手就有让人意想不到的惊喜！引领流行音乐中国风潮的他，这次邀请了诠释中国风的翘楚费玉清一起对唱新专辑的首波主打“千里...","lan":"国语","listen_count":"103833900","pubTime":"2006-09-01","score":"813","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"11","albumID":"14536","albumMID":"000OixvE1YjIqd","albumName":"霍元甲","albumtype":"EP单曲","company":"阿尔发音乐","desc":"   　江湖难测，音乐难得， \n　　擂台等着，天下谁的？！ \n　　华人音乐英雄 周杰伦 2006第一首音乐单曲＜霍元甲＞ \n　　国际武术英雄 李连杰...","lan":"国语","listen_count":"49103074","pubTime":"2006-01-20","score":"775","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"0","albumID":"60671","albumMID":"0024bjiL2aocxT","albumName":"十一月的萧邦","albumtype":"录音室专辑","company":"阿尔发音乐","desc":"2005年的夏天，周杰伦并没有如同以往的发片，这不是缺席，而是希望音乐在制作上能够更加精细，琢磨再琢磨！今年迟了一点，但，感觉却多了一点！亚洲天王周...","lan":"国语","listen_count":"163440602","pubTime":"2005-11-01","score":"882","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"0","albumID":"14316","albumMID":"000ibLEm3rEG2S","albumName":"Initial J (日本版)","albumtype":"录音室专辑","company":"索尼音乐","desc":"亚洲人气天王周杰伦正式登陆日本！Jay 最近凭电影《头文字D》在全亚洲人气急速攀升，而影片亦将于八月在日本上映。在台湾尚未出过精选辑的他，率先在东洋...","lan":"国语","listen_count":"12601442","pubTime":"2005-08-31","score":"756","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"11","albumID":"14311","albumMID":"002MAeob3zLXwZ","albumName":"J III MP3 Player","albumtype":"EP单曲","company":"阿尔发音乐","desc":"　　J Ⅲ MP3 机是至目前为止，唯一一件由周杰伦直接参与外型设计和亲自担任代言人的产品。J Ⅲ 外型简约时尚，完全表现出周杰伦既酷且型的性格。J...","lan":"国语","listen_count":"19701923","pubTime":"2005-06-24","score":"750","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"1","albumID":"14323","albumMID":"0032ezFm3F53yO","albumName":"周杰伦 2004 无与伦比 演唱会 Live CD","albumtype":"现场专辑","company":"杰威尔音乐有限公司","desc":"四万人潮共襄盛举的撼人气势，百万日本火柱特效的首度登场。\n百万屏幕视效动画的迷幻舞台，温岚 + 南拳妈妈的焚身热放。\n\n2004 周杰伦『无与伦比』...","lan":"国语","listen_count":"14398749","pubTime":"2005-01-20","score":"784","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"0","albumID":"20612","albumMID":"003DFRzD192KKD","albumName":"七里香","albumtype":"录音室专辑","company":"阿尔发音乐","desc":"    周杰伦这次引用了诗人席幕蓉名诗《七里香》作为新专辑名称，周杰伦以往每一次的专辑名称都给了歌迷许多想象空间，也给了大家许多惊叹号。这次也许并不...","lan":"国语","listen_count":"126415270","pubTime":"2004-08-03","score":"829","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"11","albumID":"8221","albumMID":"001BGzMs369FzU","albumName":"寻找周杰伦","albumtype":"EP单曲","company":"杰威尔音乐有限公司","desc":"收录电影《寻找周杰伦》电影主题曲《轨迹》及插曲《断了的弦》，另外更附赠《叶惠美》专辑中11首歌曲MV，回馈喜爱杰伦的歌迷朋友们！...","lan":"国语","listen_count":"67631972","pubTime":"2003-11-01","score":"784","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"0","albumID":"8220","albumMID":"000MkMni19ClKG","albumName":"叶惠美","albumtype":"录音室专辑","company":"杰威尔音乐有限公司","desc":"2003年最被期待的专辑－周杰伦第四张全新国语大碟《叶惠美》，在万众期待之下，确定于7月31隆重发行。 　　\n周杰伦2003年夏天全新大碟《叶惠美》...","lan":"国语","listen_count":"144693526","pubTime":"2003-07-31","score":"834","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"1","albumID":"14320","albumMID":"001O06fF2b3W8P","albumName":"The One演唱会","albumtype":"现场专辑","company":"杰威尔音乐有限公司","desc":"周杰伦「THE ONE」演唱会影音产品系列，最后一次收藏机会的「THE ONE演唱会LIVE DVD」，在唱片公司的要求高质量的原则下，不惜以长达两...","lan":"国语","listen_count":"2512664","pubTime":"2002-10-01","score":"767","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"0","albumID":"8219","albumMID":"004MGitN0zEHpb","albumName":"八度空间","albumtype":"录音室专辑","company":"杰威尔音乐有限公司","desc":"在金曲奖上大放异彩的音乐奇才周杰伦，今年破天荒地一连拿下多项大奖，可说是缔造华语歌坛前所未有的新纪录，这样傲人的成绩，不仅再度肯定杰伦音乐上过人的才...","lan":"国语","listen_count":"114396190","pubTime":"2002-07-18","score":"826","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"11","albumID":"60736","albumMID":"003dAEX43IElBh","albumName":"范特西PLUS","albumtype":"EP单曲","company":"杰威尔音乐有限公司","desc":"周杰伦的Fantasy Plus，除了之前发行的Fantasy之外，另外收录了5首歌曲\n《蜗牛》是周杰伦早期的作品，最初由上华众星演唱。是一首公益歌...","lan":"国语","listen_count":"67504498","pubTime":"2001-12-28","score":"868","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"1","albumID":"14319","albumMID":"00474nNC0yEs3T","albumName":"Fantasy Show香港演唱会","albumtype":"现场专辑","company":"Self-Released","desc":"周杰伦在11月4、5两日在香港红勘体育馆举行了两场演唱会，该演唱会门票在半个小时内就卖光。香港歌迷反映超热烈，甚至被形容为是香港近年来难得一见的盛况...","lan":"国语","listen_count":"2673430","pubTime":"2001-11-05","score":"763","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"0","albumID":"8217","albumMID":"000I5jJB3blWeN","albumName":"范特西","albumtype":"录音室专辑","company":"阿尔发音乐","desc":"⊙周杰伦要发片了！\n\n在这段不算短的日子中，喜爱杰伦的歌迷们，仅能透过市面上流传属于杰伦气息的旋律，来稍稍抚慰失落的心情。经过将近一年时间的精墩细熬...","lan":"国语","listen_count":"147369881","pubTime":"2001-09-20","score":"825","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0},{"Fattribute_5":"0","Ftype":"0","albumID":"8218","albumMID":"000f01724fd7TH","albumName":"Jay","albumtype":"录音室专辑","company":"阿尔发音乐","desc":"专辑中以古典弦乐伴奏及Band，形成英国式的复古风格，加上西班牙式风格的弦乐演奏，意境极度逼近电影配乐。\n\n⊙周杰伦同名专辑 杰伦\n\n\n“杰伦”同名...","lan":"国语","listen_count":"184353845","pubTime":"2000-11-07","score":"833","shoufa":1,"singerID":"4558","singerMID":"0025NhlN2yWrP4","singerName":"周杰伦","singers":[{"singer_id":"4558","singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦"}],"type":0}],"singer_id":4558,"singer_mid":"0025NhlN2yWrP4","singer_name":"周杰伦","total":30},"message":"succ","subcode":0})
```
	我们需要一个正则表达式去获取里面的json

```
searchObj = re.search('MusicJsonCallbacksinger_album\((.*)$',respons.content, re.M | re.I)
albums = searchObj.group(1)
```
	3. 然后我们需要看看json里面的结构是啥的,获取我们需要的信息.我们发现
	![image](https://github.com/fantasysea/jaychou/raw/master/pics/album.png)
```
"albumMID":"003RMaRI1iFoYd",
"albumName":"周杰伦的床边故事",
```
	这两个字段对我们有用,第一个字段和专辑详情页的url有关系,专辑详情页的url都是'https://y.qq.com/n/yqq/album/+专辑albumMID.html'这样的结构
	如下图:
	![image](https://github.com/fantasysea/jaychou/raw/master/pics/albumdetail.png)
	这样我们就可以按照这个规律去拼接没一张专辑的url来遍历了.

------------------目前我们得到了专辑的全部url--------------我是分割线------------------------------------------------


	4. 点击某一个专辑我们现在需要看看歌曲的列表哪里获取的,和第一步一样,F12找找歌名,看看哪里返回了这个歌名,然后发现
	![image](https://github.com/fantasysea/jaychou/raw/master/pics/song.png)
	'https://c.y.qq.com/v8/fcg-bin/fcg_v8_album_info_cp.fcg?albummid=003RMaRI1iFoYd&g_tk=937285759&jsonpCallback=albuminfoCallback&loginUin=261696254&hostUin=0&format=jsonp&inCharset=utf8&outCharset=utf-8&notice=0&platform=yqq&needNewCode=0'
	这个url是我们需要的.看看这个url返回了什么呢.
```
	 albuminfoCallback({"code":0,"data":{"aDate":"2016-06-24","color":5119262,"company":"杰威尔音乐有限公司","company_new":{"brief":"“杰威尔”是著名流行音乐歌手周杰伦于2007年成立的个人音乐品牌。周杰伦结束与阿尔发8年的合作，自立门户，和前阿尔发总经理杨峻荣、创作好搭档方文山携手组成“杰威尔(JVR)音乐有限公司”。新公司名字的来源是周杰伦(JAY)、方文山(VINCENT)和杨峻荣(JR)英文名的缩写“JVR”。","headPic":"http://y.gtimg.cn/music/common/upload/t_company_picture/37393.jpg","id":2317,"is_show":1,"name":"杰威尔音乐有限公司"},"cur_song_num":10,"desc":"【周杰伦全新数字专辑《周杰伦的床边故事》震撼上线！\n预购用户以及当前购买用户可收听、下载专辑内全部歌曲。】\n\n夜深了\n猫头鹰出没\n数完第一千零一个尖叫声\n开始听\n周杰伦 的床边故事\nJay Chou’s Bedtime Stories\n\n今年最令人激动的乐坛震撼弹\n周杰伦2016年最新着魔专辑强势暗袭你的床边！\n超乎想象、最自由的古典灵魂摇滚嘻哈\n听周杰伦说书！10个出神入化的音乐故事\n\n等待是值得的！2016年周杰伦终于推出全新专辑「周杰伦的床边故事」，每次出辑都颠覆大家想象的周杰伦，这次的「床边故事」，当然精采绝伦不让你睡！整张专辑就像是本有声书，由周杰伦说书，诉说10个最与众不同、充满想象的音乐故事！\n\n专辑同名主打「床边故事」，周杰伦带大家一同穿越魔幻回到童年，以古典华丽鬼爵电音舞曲营造出床边故事里的奇幻世界；猫头鹰监视着夜的动静，暗兽们与巫婆躲在森林里蠢蠢欲动；床边的衣橱里，藏着什么秘密？故事书里的城堡都变成立体的，八音盒发疯似的响个不停…；周杰伦的床边故事，将童谣「虎姑婆」的惊悚情节也融入歌词中，不同于一般印象中的温馨甜美，而是充满了暗黑奇想、幻术魔咒、神秘幽暗的种种。\n你准备好睁大眼睛、张开耳朵，一起听故事了吗？\n\n~序~\n\n黑夜  是猫头鹰的前世情人\n幻梦  是巫婆和英雄的森林\n床边  是睡不着的黑键白键\n故事  是暗兽们的告白气球\n\n这些故事  \n在床边流传了数千数百万年\n这些夜晚  \n比每一个白天还要缤纷精彩\n这些音符\n让好奇心与想象力一见钟情\n这些杰伦  \n小孩男人英雄废柴全部合体\n这些歌曲  \n打开梦结界让感觉无远弗届\n\n脑内钟响  滴答倒数\n3.. .2… 1…  催促着 \n赶快睡  听我说  \n衣橱的门  发出轻叹  缓缓打开\n乖乖睡  听我说\n\n耳朵不听使唤\n打开第一页\n灵魂烫金\n请发挥你的想象力\n\n想进入周杰伦的立体魔幻故事里面吗？\n通关密语是：『从前从前…』\n\n\n------------------------------------------------------------------\n\n\n床边故事导读：\n\n\n床边故事  词 方文山  曲 周杰伦  \n\n以前大家有费玉清唱的「晚安曲」，而现在，这首HIP HOP曲风的「床边故事」将让你的夜精彩绝伦，前奏以猫头鹰叫声和古典弦乐营造夜的神秘，音乐彷佛将故事书里的城堡变成立体的，听着听着就掉入了城堡里的奇幻世界，HIPHOP和RAP带领着大家打开衣橱、一探究竟，副歌变成快节奏嘻哈风舞曲，古典嘻哈风格的编曲里甚至还有魔怪的诡异笑声和巫婆的尖叫声！热闹非凡的床边故事，绝对是新世代的绝佳晚安曲！而周杰伦更以悬疑的语气先引诱大家的兴趣，慢慢催眠大家，到了副歌时才展开攻势，邀请大家一起同乐，一起探险故事里的奇幻世界！\n\n说走就走  词 方文山  曲 周杰伦\n诗意不羁的嘻哈乡村摇滚  说走就走的青春爱恋壮游 \n\n由斑鸠琴声引领开场，一场说走就走的旅行，杰伦式的嘻哈摇滚，加入的乡村摇滚里又揉入英式摇滚节奏交织出一片微妙的风景、旅行的况味；在沙漠中数星星、等黎明；无边的风景，耳里呼啸的声音，也许是爱情最暧昧的声音：\n\n真爱让人拥有力量  你我都该勇敢  率性放下一切别管\n\n每个人都该趁着青春，不顾一切的说走就走，进行一场属于自己与爱情的壮游，去体会沙漠中无边无际的悲壮、去感受旷野里夜晚的被星空拥抱的幸福感；去聆听旅行中在耳边呼啸的宇宙对你说的每句话语；旅行让每个人都变成诗人，旅行也让每段青春都变得永恒；你我都该勇敢，率性放下一切别管，说走就走！\n\n一点点  词：方文山  曲：周杰伦\n当爱只剩一点点  痛却开始蔓延\n\n以大量的弦乐、编曲包覆的吶喊，是对远离的爱情，最后争取那一点点可能留下来的努力；爱情里的无奈、忽略、视而不见，习惯爱在身旁就忘了珍惜；约会总是嫌太远、吵架、忘记生日、敷衍亏欠；方文山用简单几个情节与画面，就将这些爱情里的纠结写得淋漓尽致；旋律带着温柔却隐藏着亏欠，虐心的情节在歌曲中上演，周杰伦用充满情感的声线演出\n抱紧你  却抓不住  你的爱剩  一点点\n那一天  你几乎带走我  所有  的从前\n故事里  不再出现与你相关  的画面\n一个人的幸福  怎么编\n\n过去的幸福，是爱情里每段情节一点点累积出来的，而毁灭爱情的，也是彼此一点点的忽略与亏欠；当爱要离开时，自己才恍然面对这一切，紧抓住最后那一点点的爱；听着这首歌，也许勾起一些已经走远的遗憾，也许提醒自己要珍惜现在的幸福。\n\n前世情人  词：黄俊郎  曲：周杰伦\n\n有个女孩叫Hathaway  在她四个月大时\n弹了一段旋律  我引以为傲  她 ……………………… 是我女儿\n巴洛克古典+电子迷幻嘻哈  前世与今生的美丽相遇\n\n这首歌是周杰伦的女儿，在四个月大的时候弹了一段旋律，给了他灵感，而创作出这首「前世情人」。擅长用文字营造出中古世纪巴洛克美学的黄俊郎，这次也写出了令人目眩神迷的美丽场景：\n松鼠陪着核桃 在庭院捉迷藏  葡萄躲进橡木桶 酿出时光\n夏日在玉米齿缝中游荡  我为妳准备的四季 正在生长\n炼金师从故事炼出土壤  我阖上书也闻到了花香\n草地上的妳比果实芬芳  妳就像天使手里的糖\n\n前奏以遥远的古典钢琴开始叙述这个前世的故事，有一种爱不说就已经存在，女儿就像是前世的情人，望着那张小脸蛋，就明白什么是爱；周杰伦以声乐式的吟唱副歌，像是一种誓言与许愿：\n什么爱 不说 就已经存在 什么爱 望着 就全都明白\n妳笑 一点一点一滴漾开 一字一句 形容不来 是星空上的银海\n\n我会  当妳昼夜骑士 烈阳的树荫 让花朵为妳吟游的魔术师\n每一道有妳风景 帮妳按下快门的秘密情人\n\n我后来 会在 纯白的礼堂 牵好久 的手  交给另个他\n眼泪一点一点一滴流下 感动也会跟着留下\n远远看着你们幸福  像前世我们有过的模样\n\n「前世情人」用巴洛克式的华丽古典风格，加上电子迷幻嘻哈，营造出神秘又迷人的风格来诠释，彷佛人真的穿梭回前世另外一个国度里，看着一个小女孩出生、成长、四季更迭、逐渐成长、恋爱、最后步入礼堂的短片，浓缩而精彩，而歌里蕴藏的父爱更令人动容！\n\n英雄  词：周杰伦  曲：周杰伦\n《英雄联盟》主题曲\n\n属于召唤师们的战歌  号召各界英雄  击响战鼓  全面出动\n周杰伦为英雄联盟打造品牌主题曲「英雄」，以悬疑重拍摇滚嘻哈曲风，塑造出游戏世界里的英雄们各个身怀绝技、武功高强的音乐形象！\n身为《英雄联盟》忠实玩家的周杰伦，以他独特的歌词语法将游戏对战的过程描述得活灵活现；甚至游戏术语也可以唱成歌词；还幽默地改编蔡依林「舞娘」的副歌歌词，来形容游戏里的战斗动作：\n\n旋转跳跃你闭着眼\n卡特转完会让你闭上眼\nEZ EZ 要你 easy\n小鱼人再跳 我就把你 切成生鱼片\n\n以层层堆栈的人声合音来表现集结战友们一起战斗，编曲充满紧张刺激又悬疑，急促的鼓点让人热血沸腾血脉喷张；这首主题曲非常适合搭配游戏服用，热爱《英雄联盟》的网友们，赶快奋起，接战帖吧！\n\n不该 with aMEI  曲 周杰伦 词 方文山\n周杰伦& aMEI\n地表最强情歌！华语乐坛震撼对唱！\n\n本世纪最梦幻组合，绝对超乎你的想象！周杰伦与aMEI首度情歌对唱，在歌坛上绝对是个令人惊呼的梦幻组合！周杰伦又一次成功吓疯大家！\n\n周杰伦与aMEI首度合唱新歌【不该】，两人深情诠释雪地里的浪漫爱情，唯美梦幻地不该醒来！\n第一次合作的两人，对于歌曲都很要求，也很挑剔，所以在配唱时彼此为了要达到彼此要的感觉，不断地唱、不断调整，期望作品能够达到最完美！\n这首由方文山填词、周杰伦谱曲的【不该】，是首唯美浪漫而哀伤的情歌，以钢琴与弦乐交织出这段发生在雪地里的爱情，爱情来时气势磅礡如满天纷飞的白雪，令人目眩神迷、陶醉其中梦幻：\n雪地里相爱  他们说零下已结晶  的誓言  不会坏\n但爱的状态  却不会永远都冰封  而透明  的存在\n许下的梦  融化的太快\n或许我们都  不该醒来\n但是爱的状态，不会永远冰封而透明的存在，即使是零下结晶的誓言，也可能会融化消逝；当爱的状态已经改变，该不该醒来？缘若尽了，也许不该重来；放开彼此的手，才能拥抱更多的未来。\n放手后爱依然在  雪融了就应该花开  缘若尽了就不该再重来\n我离开  将你的手交给下个最爱\n彼此紧握的手松开  去拥抱更多未来\n方文山以雪写爱，雪的飘忽与梦幻，让人迷惑、憧憬而期待；而雪融时，却是花开之时，描述爱虽消逝，但是等待的却可能是更多美好而辽阔的未来。爱总是很美，美在拥有彼此的瞬间，瞬间即是永恒。\n周杰伦与aMEI的合唱，不仅是创造了一首歌的爱情，更创造了这个世纪的永恒经典。\n\n土耳其冰淇淋  词：周杰伦  曲: 周杰伦\n土耳其冰淇淋般炫技舞曲  古典拉丁热情舞曲一路玩到底！\n\n舞曲和土耳其冰淇淋，到底有什么关联性？让人摸不着头绪，正是这首歌出奇不意的惊喜！每张专辑都一定会有一首巨炮担任DJ念OS的歌曲，这次就是这首「土耳其冰淇淋」，一开始的爵士钢琴加上浩室电音舞曲节奏，而杰伦的歌词开场白就告诉你舞曲和冰淇淋的关连就是……\n土耳其冰淇淋  就像是女人的心  在你的面前转来转去  却捉摸不定\n这音乐跳Waacking也可以跳Locking  我耍你耍的就像土耳其的冰淇淋\n\n谁说拍中国风  一定要配灯笼  谁说写中国风  一定要商角征羽宫\n我干脆自己下车  指挥乐坛的交通  管他管他什么曲风\n\n几个合弦、几个切分音，舞曲也可以很有音乐性；换个乐器整个音乐调性与画面就可以整个转换，就像幻术一样，捉摸不定；打破一般舞曲的规则与逻辑，在中段巧妙地加入了莫扎特的土耳其进行曲曲调，能这样把古典音乐和现代流行各种元素玩成这样的，周杰伦绝对是唯一！\n\n告白气球  词：方文山  曲：周杰伦\n恋恋巴黎香榭  浪漫R&B告白神曲\n\n全世界最浪漫、最容易让人想谈恋爱的地方，绝对是巴黎左岸！这首告白情歌，前段充满R&B情调，描述遇上爱情的悸动；但是非常特别的是，副歌之后却转成浪漫”手摇歌”，让人听着听着，跟着旋律与节奏，就忍不住一起举起双手，摇动起来！像演唱会上拿着荧光棒，全场一起浪漫！\n你说你有点难追  想让我知难而退  礼物不需挑最贵  只要香榭的落叶\n喔～营造浪漫的约会  不害怕搞砸一切  拥有你就拥有  全世界\n亲爱的  爱上你  从那天起  甜蜜的很轻易\n亲爱的  别任性  你的眼睛  在说我愿意\n方文山用气球来描述女生说很难追的小趣味，让这首告白情歌的情节更浪漫；爱情总是发生在这些美好的小细节上；每个准备要告白的人，别怕爱说不出口，让这首歌给自己一点勇气，趁着气氛浪漫，告白吧！\n\nNow You See Me  词：方文山  曲：周杰伦\n《出神入化2》全球主题曲   全世界都得听他的\n\n周杰伦2011年进军好莱坞演出处女作《青蜂侠The GreenHornet》后，再次演出《出神入化2 Now You See Me 2》！电影《出神入化Now You See Me》在全球带来一阵魔术风潮，周董应邀客串续集《出神入化2 Now You See Me 2 》，周杰伦更特地为电影创作同名主题曲「Now You See Me」，这是好莱坞第一次以中文作为全球的电影主题曲，周杰伦以融合了中国风与异国风的魔幻音乐来创造出电影里的”出神入化”\n出神入化  雾里看花  跟我拼命你会输掉所有筹码\n出神入化  浪迹天涯  认真起来我连我自己都害怕\n\n由他执导的MV也将让全球看见他在影像创作上的出神入化！这是好莱坞第一首全中文的全球电影主题曲！不仅是华语乐坛空前的骄傲！更在世界影坛上具有历史意义！从「青蜂侠」片尾曲「双截棍」，到「功夫熊猫」的全球主题曲「Try」，再到「出神入化」的这首全球主题曲「Now You See Me」，周杰伦说：「最开心的就是唱中文这件事情！」\n\n爱情废柴  词：周杰伦  曲：周杰伦\n周式失恋废柴情歌  用耍赖打败爱情的失败\n\n能当英雄，谁要当废柴？但是爱情总不给人选择的机会，往往措手不及的误解与错过，就成了爱情里的废柴，搞不懂下一步该怎么办…；前奏听似浪漫情怀的弦乐，场面却是被爱打败的狼狈画面，周杰伦直白又直击人心痛处的歌词，令人心有戚戚焉，充满共鸣：\n圣诞节  剩下单人的剩单节  过条街  最好又给我下起雪\n这么衰  这么刚好  这么狼狈  要不要拍成MV干脆\n周导演总是在写歌时就把故事情节写好，一个失恋失意颓废的男人，喝了点酒带点醉意，但是想醉却醉不了，忘不了失恋的痛苦，甚至要为爱封麦！\n为你封麦  只唱你爱  Goodbye Don’t Cry  哭个痛快\n曲终人散  你也走散  我承认我是爱情里的废柴\n\n没有你的冬天，没有爱的世界，下着雪，选一首凄美的旋律，哼给自己听，也许痛到最高点，只能用耍赖才能打败爱情里的失败吧！","genre":"Pop 流行","id":1458791,"lan":"","list":[{"albumdesc":"","albumid":1458791,"albummid":"003RMaRI1iFoYd","albumname":"周杰伦的床边故事","alertid":100002,"belongCD":1,"cdIdx":0,"interval":226,"isonly":0,"label":"4611703473178673153","msgid":14,"pay":{"payalbum":1,"payalbumprice":2000,"paydownload":1,"payinfo":1,"payplay":0,"paytrackmouth":1,"paytrackprice":200,"timefree":0},"preview":{"trybegin":57890,"tryend":76722,"trysize":302183},"rate":31,"singer":[{"id":4558,"mid":"0025NhlN2yWrP4","name":"周杰伦"}],"size128":3620568,"size320":9051006,"size5_1":0,"sizeape":27544371,"sizeflac":27623859,"sizeogg":5402863,"songid":107192071,"songmid":"002MVjb31ZNK5m","songname":"床边故事","songorig":"床边故事","songtype":0,"strMediaMid":"002MVjb31ZNK5m","stream":1,"switch":636675,"type":0,"vid":"j0020a2336a"},{"albumdesc":"","albumid":1458791,"albummid":"003RMaRI1iFoYd","albumname":"周杰伦的床边故事","alertid":100002,"belongCD":2,"cdIdx":0,"interval":266,"isonly":0,"label":"5764624977789714433","msgid":14,"pay":{"payalbum":1,"payalbumprice":2000,"paydownload":1,"payinfo":1,"payplay":0,"paytrackmouth":1,"paytrackprice":200,"timefree":0},"preview":{"trybegin":68807,"tryend":98775,"trysize":480652},"rate":31,"singer":[{"id":4558,"mid":"0025NhlN2yWrP4","name":"周杰伦"}],"size128":4257051,"size320":10642491,"size5_1":0,"sizeape":34378471,"sizeflac":34460854,"sizeogg":6600758,"songid":107192072,"songmid":"003J6p942xp6ho","songname":"说走就走","songorig":"说走就走","songtype":0,"strMediaMid":"003J6p942xp6ho","stream":1,"switch":636675,"type":0,"vid":"o0020llfe2p"},{"albumdesc":"","albumid":1458791,"albummid":"003RMaRI1iFoYd","albumname":"周杰伦的床边故事","alertid":100002,"belongCD":3,"cdIdx":0,"interval":221,"isonly":0,"label":"4611686153723052033","msgid":14,"pay":{"payalbum":1,"payalbumprice":2000,"paydownload":1,"payinfo":1,"payplay":0,"paytrackmouth":1,"paytrackprice":200,"timefree":0},"preview":{"trybegin":74867,"tryend":102655,"trysize":445543},"rate":31,"singer":[{"id":4558,"mid":"0025NhlN2yWrP4","name":"周杰伦"}],"size128":3541572,"size320":8853343,"size5_1":0,"sizeape":22448984,"sizeflac":22459478,"sizeogg":4843771,"songid":107192073,"songmid":"002lW4Yl3ylM02","songname":"一点点","songorig":"一点点","songtype":0,"strMediaMid":"002lW4Yl3ylM02","stream":1,"switch":636675,"type":0,"vid":""},{"albumdesc":"","albumid":1458791,"albummid":"003RMaRI1iFoYd","albumname":"周杰伦的床边故事","alertid":100002,"belongCD":4,"cdIdx":0,"interval":200,"isonly":0,"label":"4611703473178673153","msgid":14,"pay":{"payalbum":1,"payalbumprice":2000,"paydownload":1,"payinfo":1,"payplay":0,"paytrackmouth":1,"paytrackprice":200,"timefree":0},"preview":{"trybegin":43640,"tryend":70423,"trysize":429661},"rate":31,"singer":[{"id":4558,"mid":"0025NhlN2yWrP4","name":"周杰伦"}],"size128":3212640,"size320":8030862,"size5_1":0,"sizeape":21855075,"sizeflac":22006651,"sizeogg":4558047,"songid":107192074,"songmid":"000cFsFl1o17jp","songname":"前世情人","songorig":"前世情人","songtype":0,"strMediaMid":"000cFsFl1o17jp","stream":1,"switch":636675,"type":0,"vid":"b0020d8wsqm"},{"albumdesc":"《英雄联盟》中国品牌主题曲","albumid":1458791,"albummid":"003RMaRI1iFoYd","albumname":"周杰伦的床边故事","alertid":100002,"belongCD":5,"cdIdx":0,"interval":201,"isonly":0,"label":"4611703473182867457","msgid":14,"pay":{"payalbum":1,"payalbumprice":2000,"paydownload":1,"payinfo":1,"payplay":0,"paytrackmouth":1,"paytrackprice":200,"timefree":0},"preview":{"trybegin":48388,"tryend":81126,"trysize":524955},"rate":31,"singer":[{"id":4558,"mid":"0025NhlN2yWrP4","name":"周杰伦"}],"size128":3229354,"size320":8072685,"size5_1":0,"sizeape":25668965,"sizeflac":25841427,"sizeogg":4513769,"songid":107192075,"songmid":"0040jaL23QceqJ","songname":"英雄","songorig":"英雄","songtype":0,"strMediaMid":"0040jaL23QceqJ","stream":1,"switch":636675,"type":0,"vid":""},{"albumdesc":"《幻城》电视剧主题曲","albumid":1458791,"albummid":"003RMaRI1iFoYd","albumname":"周杰伦的床边故事","alertid":100002,"belongCD":6,"cdIdx":0,"interval":290,"isonly":0,"label":"5764607523038429185","msgid":14,"pay":{"payalbum":1,"payalbumprice":2000,"paydownload":1,"payinfo":1,"payplay":0,"paytrackmouth":1,"paytrackprice":200,"timefree":0},"preview":{"trybegin":100712,"tryend":154691,"trysize":864756},"rate":31,"singer":[{"id":4558,"mid":"0025NhlN2yWrP4","name":"周杰伦"}],"size128":4643334,"size320":11607578,"size5_1":0,"sizeape":32586457,"sizeflac":32743041,"sizeogg":6430274,"songid":107192076,"songmid":"000sxzol11raSd","songname":"不该 (with aMEI)","songorig":"不该 (with aMEI)","songtype":0,"strMediaMid":"000sxzol11raSd","stream":1,"switch":636675,"type":0,"vid":"d0020okmb5a"},{"albumdesc":"","albumid":1458791,"albummid":"003RMaRI1iFoYd","albumname":"周杰伦的床边故事","alertid":100002,"belongCD":7,"cdIdx":0,"interval":195,"isonly":0,"label":"4611703473182867457","msgid":14,"pay":{"payalbum":1,"payalbumprice":2000,"paydownload":1,"payinfo":1,"payplay":0,"paytrackmouth":1,"paytrackprice":200,"timefree":0},"preview":{"trybegin":51163,"tryend":82378,"trysize":500296},"rate":31,"singer":[{"id":4558,"mid":"0025NhlN2yWrP4","name":"周杰伦"}],"size128":3133232,"size320":7832105,"size5_1":0,"sizeape":25071973,"sizeflac":25107733,"sizeogg":4900779,"songid":107192077,"songmid":"003uYI3j4EDf5q","songname":"土耳其冰淇淋","songorig":"土耳其冰淇淋","songtype":0,"strMediaMid":"003uYI3j4EDf5q","stream":1,"switch":636675,"type":0,"vid":"a0021wpzce7"},{"albumdesc":"","albumid":1458791,"albummid":"003RMaRI1iFoYd","albumname":"周杰伦的床边故事","alertid":100002,"belongCD":8,"cdIdx":0,"interval":215,"isonly":0,"label":"4611686018435776513","msgid":14,"pay":{"payalbum":1,"payalbumprice":2000,"paydownload":1,"payinfo":1,"payplay":0,"paytrackmouth":1,"paytrackprice":200,"timefree":0},"preview":{"trybegin":65138,"tryend":85421,"trysize":325589},"rate":31,"singer":[{"id":4558,"mid":"0025NhlN2yWrP4","name":"周杰伦"}],"size128":3443771,"size320":8608939,"size5_1":0,"sizeape":24929083,"sizeflac":24971563,"sizeogg":5001304,"songid":107192078,"songmid":"003OUlho2HcRHC","songname":"告白气球","songorig":"告白气球","songtype":0,"strMediaMid":"003OUlho2HcRHC","stream":1,"switch":636675,"type":0,"vid":"u00222le4ox"},{"albumdesc":"《惊天魔盗团2》电影全球主题曲","albumid":1458791,"albummid":"003RMaRI1iFoYd","albumname":"周杰伦的床边故事","alertid":100002,"belongCD":9,"cdIdx":0,"interval":174,"isonly":0,"label":"5764624977798103041","msgid":14,"pay":{"payalbum":1,"payalbumprice":2000,"paydownload":1,"payinfo":1,"payplay":0,"paytrackmouth":1,"paytrackprice":200,"timefree":0},"preview":{"trybegin":62194,"tryend":82224,"trysize":321409},"rate":31,"singer":[{"id":4558,"mid":"0025NhlN2yWrP4","name":"周杰伦"}],"size128":2785924,"size320":6964471,"size5_1":0,"sizeape":22547437,"sizeflac":22659426,"sizeogg":4002799,"songid":107192079,"songmid":"00376COL0nP51S","songname":"Now You See Me","songorig":"Now You See Me","songtype":0,"strMediaMid":"00376COL0nP51S","stream":1,"switch":636675,"type":0,"vid":"z0020528yij"},{"albumdesc":"","albumid":1458791,"albummid":"003RMaRI1iFoYd","albumname":"周杰伦的床边故事","alertid":100002,"belongCD":10,"cdIdx":0,"interval":285,"isonly":0,"label":"5764607523038429185","msgid":14,"pay":{"payalbum":1,"payalbumprice":2000,"paydownload":1,"payinfo":1,"payplay":0,"paytrackmouth":1,"paytrackprice":200,"timefree":0},"preview":{"trybegin":65236,"tryend":113189,"trysize":768208},"rate":31,"singer":[{"id":4558,"mid":"0025NhlN2yWrP4","name":"周杰伦"}],"size128":4571007,"size320":11427210,"size5_1":0,"sizeape":31739055,"sizeflac":31629866,"sizeogg":6341196,"songid":107192080,"songmid":"000eskGX0ijIFi","songname":"爱情废柴","songorig":"爱情废柴","songtype":0,"strMediaMid":"000eskGX0ijIFi","stream":1,"switch":636675,"type":0,"vid":"m0020yw1p9v"}],"mid":"003RMaRI1iFoYd","name":"周杰伦的床边故事","singerid":4558,"singermblog":null,"singermid":"0025NhlN2yWrP4","singername":"周杰伦","song_begin":0,"total":10,"total_song_num":10},"message":"succ","subcode":0})
```
	我们需要正则表达式获取中间的json
```
	searchObj = re.search("getSongInfo : (.*),.*",respons.content)
	    if searchObj:
	        songcents = searchObj.group(1)
```
	我们看看这个结构是怎么样的,格式化一下json
	![image](https://github.com/fantasysea/jaychou/raw/master/pics/songid.png)
	我们研究了发现这几个字段比较有用
	> songmid用来拼接歌曲的详情页  'https://y.qq.com/portal/album/'+songmid+'.html'
	> songmid用来拼接歌曲的歌词url
	`https://c.y.qq.com/lyric/fcgi-bin/fcg_query_lyric.fcg?nobase64=1&callback=jsonp1&g_tk=1171713782&jsonpCallback=jsonp1&loginUin=261696254&hostUin=0&format=jsonp&inCharset=utf8&outCharset=utf-8&notice=0&platform=yqq&needNewCode=0&musicid='+str(songid)
    `
	> songname和albumname用来保存歌词到本地的时候命名文件
	*这里有一个需要注意的,本来我们获取了歌词url就不需要歌曲的详情页这个url了.但是我们发现请求歌词url的时候请求头需要歌曲url这个字段.*
	5. 我们来看看歌词url都返回了啥吧
```
	jsonp1({"retcode":0,"code":0,"subcode":0,"type":1,"songt":0,"lyric":"[ti&#58;一点点]&#10;[ar&#58;周杰伦]&#10;[al&#58;周杰伦的床边故事]&#10;[by&#58;]&#10;[offset&#58;0]&#10;[00&#58;01&#46;10]一点点&#32;&#45;&#32;周杰伦&#10;[00&#58;04&#46;79]&#10;[00&#58;06&#46;29]词：方文山&#10;[00&#58;08&#46;61]&#10;[00&#58;09&#46;70]曲：周杰伦&#10;[00&#58;11&#46;66]&#10;[00&#58;33&#46;45]几次争辩&#32;几次的失眠&#10;[00&#58;38&#46;03]&#10;[00&#58;39&#46;38]而我却&#32;视而不见&#10;[00&#58;42&#46;42]&#10;[00&#58;43&#46;28]从不&#32;先说抱歉&#10;[00&#58;46&#46;75]&#10;[00&#58;47&#46;65]工作优先&#32;跟朋友见面&#10;[00&#58;52&#46;65]&#10;[00&#58;53&#46;46]永远多过给你&#32;的时间&#10;[00&#58;59&#46;14]&#10;[01&#58;01&#46;90]那亏欠&#32;一直&#32;放在心里面&#10;[01&#58;06&#46;34]&#10;[01&#58;07&#46;58]没能说出口&#32;你已走远&#10;[01&#58;14&#46;86]抱紧你&#32;却抓不住&#32;你的爱剩&#32;一点点&#10;[01&#58;21&#46;56]&#10;[01&#58;22&#46;12]那一天&#32;你几乎带走我&#32;所有&#32;的从前&#10;[01&#58;29&#46;22]你说我&#32;根本没有因为爱你&#32;而改变&#10;[01&#58;36&#46;82]我始终听不进&#32;你的劝&#10;[01&#58;42&#46;65]&#10;[01&#58;59&#46;36]想吃甜点&#32;想去电影院&#10;[02&#58;04&#46;33]&#10;[02&#58;05&#46;45]而我却&#32;都嫌太远&#10;[02&#58;08&#46;29]&#10;[02&#58;09&#46;13]你在&#32;委曲埋怨&#10;[02&#58;12&#46;21]&#10;[02&#58;13&#46;64]我连生日&#32;都忘记敷衍&#10;[02&#58;18&#46;80]&#10;[02&#58;19&#46;45]却只会要你乖&#32;一点点&#10;[02&#58;24&#46;61]&#10;[02&#58;27&#46;85]那亏欠&#32;一直&#32;放在心里面&#10;[02&#58;32&#46;46]&#10;[02&#58;33&#46;25]没能说出口&#32;你已走远&#10;[02&#58;40&#46;86]抱紧你&#32;却抓不住&#32;你的爱剩&#32;一点点&#10;[02&#58;48&#46;03]那一天&#32;你几乎带走我&#32;所有&#32;的从前&#10;[02&#58;55&#46;21]故事里&#32;不再出现与你相关&#32;的画面&#10;[03&#58;02&#46;81]一个人的幸福&#32;怎么编&#10;[03&#58;07&#46;99]&#10;[03&#58;09&#46;61]面对面&#32;泪滑落在彼此的脸&#32;那瞬间&#10;[03&#58;16&#46;68]&#10;[03&#58;17&#46;24]不再让你离开&#32;我身边"})
```
	我们需要一个正则获取里面的json
```
	searchObj = re.search("jsonp1\((.*)\)", respons.content, re.M | re.I)
    yric = searchObj.group(1)
```
	![image](https://github.com/fantasysea/jaychou/raw/master/pics/lrcdetail.png)
	获取`lyric`并保存到本地
	![image](https://github.com/fantasysea/jaychou/raw/master/pics/songs.png)
	6. 最后获取本地的所有歌词,清洗干净那些特殊字符,然后用jieba分一下词,最后通过WordCloud输出一下.
	7. 最终会再本地生成一个文字云
	
	
	
