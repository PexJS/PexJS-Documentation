Know-how
===============================

解像度の調整を行う
********************************************

Screener 等のフルスクリーン化ユーティリティを使わず自分で解像度調整を行う場合の一例を示します。

iPhone 3GS 等のノーマルディスプレイを持つデバイスで 320px、
高画質 Android 端末や iPhone4 など高密度ディスプレイを持つデバイスに対しては 640px で描画を行う場合、例えば以下のような設定で可能となります。

.. code-block:: html

	<div class="highres"><canvas id="canvas"></canvas></div>

.. code-block:: css

	@media 
	only screen and (-webkit-min-device-pixel-ratio: 1.5) {
		.highres > canvas {
			zoom: 50%;
		}
	}

.. code-block:: javascript

	var ratio = window.devicePixelRatio >= 1.5 ? 2:1;
	new Pex('path_to_swf', 'canvas', {
		"width": 320 * ratio
		// ... その他オプション ...
	});

iOS4 + iPhone4 のように条件に該当するものの 640px での描画が苦しい端末がある場合はサーバ側でフラグを立てて意図的に 320px 描画に変更することが可能です。

.. code-block:: html

  <!-- クラス属性を外すことで CSS セレクタが変わり zoom:50% の適用が外れる -->
	<div><canvas id="canvas"></canvas></div>

.. code-block:: javascript

	var ratio = window.devicePixelRatio >= 1.5 ? 2:1;
	ratio = 1; // ここでフラグにより ratio の数値を上書き変更する
	new Pex('path_to_swf', 'canvas', {
		"width": 320 * ratio
		// ... その他オプション ...
	});



描画速度を最適化する
**********************

method オプションを用いて描画方法を指定する事でパフォーマンスが変わります。
将来的には SWF の特性に適した method 設定を選択する為のプロファイラツールを用意する予定です。

詳しくはオプション (:doc:`option`) の解説を御覧ください。


縮小を避け画質を適切に保つ
********************************************

.. #285

JPEG や PNG を内包する SWF を表示する場合、ブラウザのアンチエイリアスの仕様により画像の表示が荒れる場合があります。
特に元の SWF よりも縮小して表示させる場合に、かつ Android 端末では顕著に現れます。
上記現象に出くわした場合は Flash 側で画像サイズを調整し、ご対応ください。


フリック阻害を緩和する
********************************************

.. #232 ガンロワにて、Pex によりフリック体感が重くなる

スクロールダウン等、画面の操作につっかかりが生じる場合があります。

線数の多いオブジェクトを扱う際に Pex が計算資源を奪ってしまうことが原因であると考えられます。
このような場合は method:'func'（Pex のデフォルトオプション）に換えて method:'cache' をお試しください。

重いな、と思ったら
********************************************

もし「重いな」と思ったら、次のオプションを是非試してみてください。

	var option = { 'partialDraw': true };

	var option = { 'shapeDetail': { 'all': { 'method':'cache' } } };

	var option = { 'partialDraw': true, 'shapeDetail': { 'all': { 'method':'cache' } } };

ブラウザが落ちた場合
********************************************

もしブラウザ全体が落ちる場合は、メモリ不足が考えられます。次のオプションを試してみてください。

	var option = { 'partialDraw': false }; // デフォルトの設定

	var option = { 'shapeDetail': { 'all': { 'method':'func' } } }; // デフォルトの設定

	var option = { 'cacheColoredImage': false };

連続した SWF の再生で落ちる場合は適宜 destroy によりメモリ解放を行なってください。
ページ遷移を挟む SWF で落ちる場合は次のようなコードで改善する場合があります。

.. code-block:: javascript

	window.onunload = function() {
	  pex.getAPI().destroy();
	};


タップが反応しない場合
********************************************

タップが反応しない場合は、次のオプションを確認してください。

	var option = { 'enableTouch': true, 'enableButton': true };

このオプションをつけない限り、タップを無視する設定になっております。


movieClip の拡大操作時に画質が劣化する
********************************************

method:cache 利用時にこの問題が発生する可能性があります。

Pex は描画パフォーマンス確保の為、一度生成したキャッシュを使いまわす方針を取っており、拡大操作を行う際に画質が劣化する場合があります。
例としては下記のようになります。

- Flash 演出: まず 20x20 のシンボルを 80x80 まで拡大操作する
- Pex 処理: 20x20 でキャッシュが作成され、その後 80x80 まで拡大される（キャッシュの再生成を行わないため、画質劣化が起こる）

回避方法

- cacheScale を設定する（オプション (:doc:`option`) の解説を御覧ください）
- method:cache の利用をやめる
- Flash 側を作り変える（80x80 でシンボルを作成する）


主要 MovieClip には名前を付ける
********************************************

Pex では主要な MovieClip に対し設定を行うことが多々あります。
MovieClip に名前が付いている場合は名前を用いますが、そうでない場合は識別に ID が用いられます。

ID はサーバ側での動的合成等により変わることがある為、
上記を織り込んで主要な MovieClip には名前を付けることをお勧め致します。


画面にローディング画像を設定する
********************************************

画面にローディング画像を設定するには `getAPI().ready() <api.html#ready>`_ メソッドを使用します.

あらかじめローディング画像を表示しておき, `getAPI().ready() <api.html#ready>`_ メソッドで登録した
関数内でローディング画像を削除することで設定できます.

.. code-block:: javascript

            var pex = new Pex("./example.swf", "container", option);
            
            pex.getAPI().ready(function() {
                // ローディング画像要素を取得
                var elm = document.getElementById("loading-img");
                // 要素を不可視化
                elm.style.visibility = "hidden";
            });


Chrome for Android 着色問題について
********************************************

Chrome for Android の問題により着色処理が正常に行われず、画像が単色に塗りつぶされる場合があります。
Chrome にて画像関連の問題が発生する場合は着色処理を除去してお試しください。

また本件は着色対象のサイズが 256 x 256 を上回る場合に発生し、これ以下のサイズに収めて再生することで問題原因の切り分けが可能です。

