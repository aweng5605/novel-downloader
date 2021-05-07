# 小说下载器

一个**可扩展的**通用型小说下载器。
## 使用方法

**特别提醒：本脚本与[Greasemonkey](https://addons.mozilla.org/firefox/addon/greasemonkey/)脚本管理器不兼容。本脚本执行下载任务时将播放无声音频，以保证脚本后台运行时不被休眠。**

如果本脚本支持该小说网站，当打开小说目录页时，网页右上角会出现下载图标，点击该图标即可开始下载。

如果你要下载的小说章节较多，等待时间可能较长，此时请耐心等待。

你通过右下角进度条了解当前下载进度，或者按下 F12，打开网页控制台查看当前下载状态。

下载完成后，本脚本将会自动下载一个TXT文档及由HTML文件及图片组成的ZIP压缩包。

TXT文档请使用记事本或其它阅读软件进行阅读。

ZIP压缩包，请在解压后，直接双击打开HTML文件（`ToC.html` 为目录文件）进行阅读。
## 目前支持小说网站

**特别提醒：如欲下载支持列表中网站的付费章节，请登录相应网站帐户，并确定已购买相应付费章节。未登录网站帐户，或未购买的付费章节，下载时将直接忽略，无法进行下载。**

|站点|公共章节|付费章节|备注|
|---|-------|------|----|
|[刺猬猫](https://www.ciweimao.com/)|✅|✅\*|\*VIP章节仅支持图片版。|
|[SF轻小说](https://book.sfacg.com/)|✅\*|✅\*\*|\*不支持对话小说，例：[224282](https://book.sfacg.com/Novel/224282/)。 \*\*VIP章节仅支持图片版。|
|[起点中文网](https://book.qidian.com/)|✅|✅||
|[起点女生网](https://www.qdmm.com/)|✅|✅||
|[晋江文学城](http://www.jjwxc.net/)|✅|✅\*|\*VIP章节已使用[防盗字体对照表](https://github.com/7325156/jjwxcNovelCrawler/tree/master/%E5%8F%8D%E7%88%AC%E8%99%AB%E5%AF%B9%E7%85%A7%E8%A1%A8)去除空格，如在使用中发现VIP章节仍存在空格，请附上所下载的文件进行反馈。|
|[长佩文学](https://www.gongzicp.com/)|✅|✅|反爬较严，限制下载速度，每分钟约可下载12章，请耐心等待。|
|[轻之文库轻小说](https://www.linovel.net/)|✅|❌|VIP章节仅支持APP查看|
|[纵横中文网](http://www.zongheng.com/)|✅|❌||
|[花语女生网](http://huayu.zongheng.com/)|✅|❌||
|[17K小说网](https://www.17k.com/)|✅|❌||
|[书海小说网](http://www.shuhai.com/)|✅|❌||
|[塔读文学](https://www.tadu.com/)|✅|❌||
|[UU看书网](https://www.uukanshu.com/)|✅|❎||
|[亿软网](http://www.yruan.com/)|✅|❎||
|[笔趣窝](http://www.biquwoo.com/)|✅|❎||
|[书趣阁](http://www.shuquge.com/)|✅|❎||
|[顶点小说](https://www.dingdiann.net/)|✅|❎||
|[星空中文](http://www.xkzw.org/)|✅|❎||
|[乐文小说网](https://www.lewenn.com/)|✅|❎||
|[266看书](https://www.266ks.com/)|✅|❎||
|[和图书](https://www.hetushu.com/index.php)|✅|❎||
|[手打吧](https://www.shouda88.com/)|✅|❎||
|[阁笔趣](http://www.gebiqu.com/)|✅|❎||
|[米趣小说](http://www.viviyzw.com/)|✅|❎||
|[书书网](https://www.xiaoshuodaquan.com/)|✅|❎||
|[八一中文网](https://www.81book.com/)|✅|❎||
|[御书阁](http://m.yuzhaige.cc/)|✅|❎|部分文字被图片替换，请使用HTML版查看。|
|[完本神站](https://www.xinwanben.com/)|✅|❎||
|[得间小说](https://www.idejian.com/)|✅|❎||

## 高阶使用技巧

### 自定义筛选函数

如欲只下载部分章节，请在点击运行按钮前，按下 F12 打开开发者工具，在 `Window` 下创建自定义筛选函数 `chapterFilter` 。

```typescript
class Chapter {
    bookUrl: string;
    bookname: string;
    chapterUrl: string;
    chapterNumber: number;
    chapterName: string | null;
    isVIP: boolean;
    isPaid: boolean | null;
    sectionName: string | null;
    sectionNumber: number | null;
    sectionChapterNumber: number | null;
    chapterParse: ruleClassNamespace.chapterParse;
    charset: string;
    status: Status;
    retryTime: number;
    contentRaw: HTMLElement | null;
    contentText: string | null;
    contentHTML: HTMLElement | null;
    contentImages: attachmentClass[] | null;
    constructor(bookUrl: string, bookname: string, chapterUrl: string, chapterNumber: number, chapterName: string | null, isVIP: boolean, isPaid: boolean | null, sectionName: string | null, sectionNumber: number | null, sectionChapterNumber: number | null, chapterParse: ruleClassNamespace.chapterParse, charset: string);
    init(): Promise<chapterParseObject>;
    private parse;
}

interface chapterFilter {
    (chapter: Chapter): boolean;
}
```

**自定义筛选函数示例：**

只下载该本小说前100章内容：

```javascript
function chapterFilter(chapter) {
  return chapter.chapterNumber <= 100
}
```

只下载第一卷内容：

```javascript
function chapterFilter(chapter) {
  return chapter.sectionNumber === 1
}
```

只下载章节名称中含有“武器”的章节：

```javascript
function chapterFilter(chapter) {
  return chapter.chapterName.includes("武器")
}
```

## 开发

根据 `ruleClass` 接口实现相应网站解析规则 Class，并在 `rules.ts` 中添加相应选择规则。

```typescript
interface BookAdditionalMetadate {
    cover?: attachmentClass;
    attachments?: attachmentClass[];
    tags?: string[];
    lastModified?: number;
    serires?: string;
    seriresNumber?: number;
    ids?: string[] | string;
    publisher?: string;
    languages?: string;
}
class attachmentClass {
    imageUrl: string;
    name: string;
    mode: "naive" | "TM";
    referer?: string;
    status: Status;
    retryTime: number;
    imageBlob: Blob | null;
    constructor(imageUrl: string, name: string, mode: "naive" | "TM");
    init(): Promise<Blob | null>;
    private downloadImage;
    private tmDownloadImage;
}
interface bookParseObject {
    bookUrl: string;
    bookname: string;
    author: string;
    introduction: string | null;
    additionalMetadate: BookAdditionalMetadate;
    chapters: Chapter[];
}
interface chapterParseObject {
    chapterName: string | null;
    contentRaw: HTMLElement | null;
    contentText: string | null;
    contentHTML: HTMLElement | null;
    contentImages: attachmentClass[] | null;
}
declare namespace ruleClassNamespace {
     interface bookParse {
        (): Promise<bookParseObject>;
    }
    interface chapterParse {
        (chapterUrl: string, chapterName: string | null, isVIP: boolean, isPaid: boolean | null, charset: string): Promise<chapterParseObject>;
    }
}
interface ruleClass {
    imageMode: "naive" | "TM";
    charset?: string;
    concurrencyLimit?: number;
    maxRunLimit?: number;
    bookParse(chapterParse: ruleClassNamespace.chapterParse): Promise<bookParseObject>;
    chapterParse(chapterUrl: string, chapterName: string | null, isVIP: boolean, isPaid: boolean | null, charset: string): Promise<chapterParseObject>;
}
```

## License

AGPL-3.0
