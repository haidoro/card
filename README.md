# カードを回転させる仕組み

## 回転の基本的な原理（commit:cardの裏表作成）
簡単な赤の300 x 500pxのカードを作成します。

```
<div class="card">
  <div class="front">front</div>
</div>
```

CSSは次のようにします。

```
.card{
  width:300px;
  height:500px;
}
.front{
  background-color: red;
  height:100%;
}
```

このカードを回転させるには、transform:rotateY(180deg)を使用します。
360度回転すると元の状態になりますので、半分の回転180度とします。

```
.card:hover{
  transform:rotateY(180deg);
}
```

このままでは一瞬で回転しますので、「.card」にtrnsitionアニメーションをつけると回転の状態を確認することができます。

## カードの裏表作成
カードの裏側を作成します。
```
<div class="card">
  <div class="front">front</div>
  <div class="back">back</div><!--  裏部分追加  -->
</div>
```

裏表面のCSSは次のようになります。裏面に「transform:rotateY(180deg)」をあらかじめ入れておくことがポイントです。

```
.front{
　　background-color: red;
　　height:100%;
　　transition:.8s;
}
.back{
  background-color: blue;
  height:100%;
  transform:rotateY(180deg);
  transition:.8s;
}
```
マウスホバーで回転させる記述は以下です。

```
.card:hover .front{
  transform:rotateY(180deg);
}
.card:hover .back{
  transform:rotateY(0);
}
```

これでマウスホバーすると回転して赤のカードも青のカードも反転することがわかると思います。
欠点として回転が回転に見えずに伸縮して内容が反転したように見えることです。

## perspectiveプロパティ追加(commit:perspectiveプロパティ追加)
上記の欠点を解決するには遠近感をつけることです。
そのために「perspective」プロパティを使います。これは「.card」に設定します。

***perspective プロパティは、3D 配置された要素に遠近感を与えるため、z=0 平面とユーザ間の距離を決めます。***

```
.card{
  perspective:150rem;
  -moz-perspective: 150rem;
  width:300px;
  height:500px;
}
```

## 表面と裏面を重ねる（commit:裏表のcardを重ねた）
表面と裏面を重ねるには、それぞれに「position:absolute」をかけて同じ座標にするだけです。
親要素に「position:relative」を忘れないようにします。

```
.front{
  background-color: red;
  height:100%;
  width:100%;
  transition:.8s;
  position: absolute;
  top: 0;
  left: 0;
}
.back{
  background-color: blue;
  height:100%;
  width:100%;
  transform:rotateY(180deg);
  transition:.8s;
  position: absolute;
  top: 0;
  left: 0;
}
```

うまく重なりましたが、動きは少し変です。

## cardの回転完成(commit:cardの回転完成)

回転の際の違和感は「backface-visibility: hidden;」で解決できます。  
このプロパティを表面と裏面に設定します。

```
.front,
.back{
  background-color: red;
  height:100%;
  width:100%;
  transition:.8s;
  position: absolute;
  top: 0;
  left: 0;
  backface-visibility: hidden;
}
.back{
  background-color: blue;
  transform:rotateY(180deg);
}
```

「backface-visibility」プロパティは要素の裏面がユーザに面したときに、裏面を可視にするかどうかを決めます。要素の裏面は常に透明な背景を持ち、可視ならば表面の鏡像を表示します。
今回のようなカードを裏返す効果を使う場合のように、要素の裏面から表面が見えてほしくない場合に使います。
