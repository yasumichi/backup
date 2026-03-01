# javascript css class 定義

JavaScriptでCSSクラスを定義・操作するには、classList APIを用いて要素のクラスを追加・削除（add/remove/toggle）するのが最適です。事前にCSSファイルでクラス（.my-class { ... }）を定義し、JSからelement.classList.add('my-class')で適用するのが基本です。

- [スタイルとクラス](https://ja.javascript.info/styles-and-classes)
- [CSSリファレンス(基礎知識) - とほほのWWW入門](https://www.tohoho-web.com/css/selector.htm)
- [CSSのclass属性とは？意味をわかりやすく簡単に解説 – / XEXEQ(ゼゼック)](https://xexeq.jp/blogs/media/it-glossary675)

## JavaScriptでCSSクラスを操作する主な方法

1. classList.add(className): CSSクラスを追加

```javascript
document.getElementById("myElement").classList.add("new-class");
```

1. classList.remove(className): CSSクラスを削除

```javascript
document.getElementById("myElement").classList.remove("old-class");
```

1. classList.toggle(className): CSSクラスが存在すれば削除、なければ追加

```javascript
document.getElementById("myElement").classList.toggle("active");
```

1. className (直接設定): クラス全体を置き換え

```javascript
document.getElementById("myElement").className = "new-class";
```

## JSでCSSルール自体を動的に定義する手法

styleタグを生成し、insertRuleで動的にスタイルをページに追加する手法もあります。

- [JavaScriptでCSSを動的に追加：styleタグとinsertRuleによるクラス定義の実例](https://mam-mam.net/javascript/css_js.html)

```javascript
const style = document.createElement('style');
style.textContent = '.dynamic-class { color: red; }';
document.head.appendChild(style);
// 後で適用
document.getElementById('myElement').classList.add('dynamic-class');
```

- 推奨: 直接styleプロパティを操作するよりも、CSSで定義したクラスをJSでclassListを使って付け外しする方が、コードの可読性とメンテナンス性が高くなります。
- 注意点: HTML要素に複数のクラスを指定する場合はスペースで区切り、CSSの優先度に従って適用されます。
