======================================
API
======================================

Pex では JavaScript からムービークリップを操作するための API を提供しています。API を利用することでムービークリップの特定のフレームに移動したり、ムービークリップを停止させたりできます。


API を利用するには、次のように Pex オブジェクトに対して getAPI メソッドを呼び出し、得られたオブジェクトに対して API を呼び出します。

.. code-block:: javascript

	var pex = new Pex("./example.swf", "container", option);
	pex.getAPI().stop();  //  stop API を呼び出す


getAPI() はシングルトンで実装されており、ひとつの Pex オブジェクトに対してひとつの API オブジェクトが対応しております。

.. code-block:: javascript

	var api = pex.getAPI(); // 同じapiを使いまわせる
	api.stop();
	api.start();


主要 API
========

gotoFrame
*********

指定したムービークリップを指定したフレームに移動させます。

..

	gotoFrame(name, frame)


+--------------+--------------------------------------------------+
| 引数名       | 概要                                             |
+==============+==================================================+
| name         | ムービークリップの名前                           |
+--------------+--------------------------------------------------+
| frame        | 移動先のフレーム番号, もしくはフレームラベル名   |
+--------------+--------------------------------------------------+


gotoAndStop
***********

指定したムービークリップを指定したフレームに移動させた上で停止させます。

..

	gotoAndStop(name, frame)


+--------------+--------------------------------------------------+
| 引数名       | 概要                                             |
+==============+==================================================+
| name         | ムービークリップの名前                           |
+--------------+--------------------------------------------------+
| frame        | 移動先のフレーム番号, もしくはフレームラベル名   |
+--------------+--------------------------------------------------+


play
****

指定したムービークリップを再開します。ActionScript の play と似た挙動です。

..

	play(name)


+--------------+-----------------------------------------------------------------+
| 引数名       | 概要                                                            |
+==============+=================================================================+
| name         | ムービークリップの名前（指定がない場合はルートムービークリップ）|
+--------------+-----------------------------------------------------------------+


stop
****

指定したムービークリップを停止します。ActionScript の stop と似た挙動です。

..

	stop(name)


+--------------+-----------------------------------------------------------------+
| 引数名       | 概要                                                            |
+==============+=================================================================+
| name         | ムービークリップの名前（指定がない場合はルートムービークリップ）|
+--------------+-----------------------------------------------------------------+

stopAll
*******

指定したムービークリップとそのムービークリップ配下の全てのムービークリップを停止します。

..

	stopAll(name)


+--------------+-----------------------------------------------------------------+
| 引数名       | 概要                                                            |
+==============+=================================================================+
| name         | ムービークリップの名前（指定がない場合はルートムービークリップ）|
+--------------+-----------------------------------------------------------------+


keyDown
*******

指定したキーコードを押下した効果が得られます。

..

	keyDown(keyCode)


+--------------+-----------------------------------------------------------------+
| 引数名       | 概要                                                            |
+==============+=================================================================+
| keyCode      | 押下するキーコード（例:「5」のキーコードは53）                  |
+--------------+-----------------------------------------------------------------+


ready
**********

SWF の再生準備が整ったタイミングで呼ばれる関数を指定します。

..

	ready(func)


+--------------+-----------------------------------------------------------------+
| 引数名       | 概要                                                            |
+==============+=================================================================+
| func         | プレイヤーの再生準備が整ったタイミングで呼ばれる関数            |
+--------------+-----------------------------------------------------------------+

次の例ではオプションの stopOnStart を true に指定した上で、SWF の再生準備が整ったタイミングで再生を開始するようにしています。
なお、Pex では SWF の再生準備が完全に整っていない状態でも最初のフレームを描画できるようになった時点で再生を開始するようになっています。

.. code-block:: javascript

	pex.getAPI().ready(function() { pex.getAPI().engineStart(); });


engineStart
***********

Pex エンジンをスタートさせます。engineStop や stopOnStart で再生を止めている場合に使用します。

..

	engineStart()



engineStop
**********

Pex エンジンをストップさせます。特定のムービークリップの再生を止めるのではなく、全体の再生を止めたい場合に使用します。再開するには engineStart を実行します。

..

	engineStop()


getVariable
***********

指定したムービークリップの変数の値を取得します。

..

	getVariable(name, variableName)


+--------------+-----------------------------------------------------------------+
| 引数名       | 概要                                                            |
+==============+=================================================================+
| name         | ムービークリップの名前                                          |
+--------------+-----------------------------------------------------------------+
| variableName | 値を取得したい変数名                                            |
+--------------+-----------------------------------------------------------------+


getVariables
************

指定したムービークリップの変数の値を取得します。

..

	getVariables(name, variableNames)


+---------------+-----------------------------------------------------------------+
| 引数名        | 概要                                                            |
+===============+=================================================================+
| name          | ムービークリップの名前                                          |
+---------------+-----------------------------------------------------------------+
| variableNames | 値を取得したい変数名の配列。指定しない場合は全ての変数          |
+---------------+-----------------------------------------------------------------+


使用例
------

.. code-block:: javascript

	pex.getAPI().setVariables("mc", {a: 1, b: "foo", c: null});
	pex.getAPI().getVariables("mc");              // => {a: 1, b: "foo", c: null}
	pex.getAPI().getVariables("mc", ["a", "c"]);  // => {a: 1, c: null}
	pex.getAPI().getVariables("mc", ["a", "d"]);  // => {a: 1}


setVariable
***********

指定したムービークリップに変数の値を設定します。

..

	setVariable(name, variableName, value)

+--------------+-----------------------------------------------------------------+
| 引数名       | 概要                                                            |
+==============+=================================================================+
| name         | ムービークリップの名前                                          |
+--------------+-----------------------------------------------------------------+
| variableName | 値を設定したい変数名                                            |
+--------------+-----------------------------------------------------------------+
| value        | 設定する値                                                      |
+--------------+-----------------------------------------------------------------+

注意点
------

ready で登録した関数内で setVariable する際は注意してください.

読み込む swf によっては 1 フレーム目の ActionScript で変数を初期化している場合があるので,
setVariable で設定した値が上書きされることがあります.


setVariables
************

指定したムービークリップに変数の値を設定します。

..

	setVariables(name, obj)

+--------------+-----------------------------------------------------------------+
| 引数名       | 概要                                                            |
+==============+=================================================================+
| name         | ムービークリップの名前                                          |
+--------------+-----------------------------------------------------------------+
| obj          | 値を設定したい変数名と値のペア                                  |
+--------------+-----------------------------------------------------------------+

使用例
------

.. code-block:: javascript

	pex.getAPI().setVariables("mc", {a: 1, b: "foo", c: null});
	pex.getAPI().getVariables("mc");  // => {a: 1, b: "foo", c: null}



destroy
*******

エンジンを破棄し、グローバル変数に関する参照を切ります。見た目は engineStop と同じですが、二度と再開することはできません。
実際にメモリが開放されるかどうかはブラウザの実装によります。

.. code-block:: javascript

	destroy()


getTotalFrames
*****************

指定したムービークリップのフレーム数を取得します。

..

	getTotalFrames(name)

+--------------+------------------------------------------+
| 引数名       | 概要                                     |
+==============+==========================================+
| name         | ムービークリップの名前                   |
+--------------+------------------------------------------+

.. code-block:: javascript

    pex.getAPI().getTotalFrames('/foo/bar/to/MC');


getCurrentFrame
*****************

指定したムービークリップの再生中のフレーム位置を数値として取得します.

..

	getCurrentFrame(name)

+--------------+------------------------------------------+
| 引数名       | 概要                                     |
+==============+==========================================+
| name         | ムービークリップの名前                   |
+--------------+------------------------------------------+

.. code-block:: javascript

    pex.getAPI().getCurrentFrame('/foo/bar/to/MC');


getRenderingContext
*******************

描画に使用しているCanvasのcontextオブジェクトを返します。

..

	getCurrentFrame()

.. code-block:: javascript

    var ctx = pex.getAPI().getRenderingContext();

Canvas自体への参照は、ctx.canvasを参照してください。

注意点
------
起動直後など、まだCanvasが作成されていない場合は取得することが出来ませんのでご注意ください。


replaceMovieClip
*****************

指定したムービークリップを画像に差し替えます。引数の内訳はoptionのreplaceと同じです。

..

	replaceMovieClip(name, img, width, height, keepAspect, xratio, yratio)


+--------------+------------------------------------------+
| 引数名       | 概要                                     |
+==============+==========================================+
| name         | ムービークリップの名前                   |
+--------------+------------------------------------------+
| img          | 置換する画像エレメント                   |
+--------------+------------------------------------------+
| width        | 置換先の幅                               |
+--------------+------------------------------------------+
| height       | 置換先の高さ                             |
+--------------+------------------------------------------+
| keepAspect   | アスペクト比の保存                       |
+--------------+------------------------------------------+
| xratio       | 基準点の x の位置                        |
+--------------+------------------------------------------+
| yratio       | 基準点の y の位置                        |
+--------------+------------------------------------------+

基準点のデフォルトは画像の左上です。(xratio, yratio) にはそれぞれ0から1の値を指定します。左上が (0, 0)、右下が (1, 1) です。



.. code-block:: javascript

    pex.getAPI().replaceMovieClip("test2", img, 200, 100, true);

このAPIは値を返しません

注意点
------
このAPIを使うと動的にMCを画像に差し替えることが出来ます。
実行されたタイミングで表示されているMCに該当の名前が存在した場合はその場で置き換わります。
また、既に存在する MovieClip に加え、replaceMovieClip の呼び出し後に登場する MC も置換対象となります。

その他の機能はoptionのreplaceと同じです。細かい説明は、Optionsのreplaceの項目をご参照ください。



getMovieClipNames
*****************

指定したMCが子要素として持つMC名の一覧を配列として取得します。

..

	getMovieClipNames(name)

+--------------+------------------------------------------+
| 引数名       | 概要                                     |
+==============+==========================================+
| name         | ムービークリップの名前                   |
+--------------+------------------------------------------+

.. code-block:: javascript

    pex.getAPI().getMovieClipNames("test");

上記例では、testという名前のMC内に子要素として存在するMCの名前一覧を取得します。
引数のMC名を省略した場合はルート直下が指定された事となります。


getMovieClipNamesAtPoint
************************

指定した位置に存在する全てのムービークリップの名前を取得します。
例えば SWF のフレームサイズが 240x320 px の場合、右下の角に位置するムービークリップの名前を取得するには (x, y) に (240, 320) を指定します。オプションの width や height には影響を受けません。

..

	getMovieClipNamesAtPoint(x, y)

+--------------+------------------------------------------+
| 引数名       | 概要                                     |
+==============+==========================================+
| x            | x 座標のピクセル値                       |
+--------------+------------------------------------------+
| y            | y 座標のピクセル値                       |
+--------------+------------------------------------------+






getFrameLabelMap
****************

指定したMCのフレームラベル情報をmap形式のオブジェクトで取得します。

..

	getFrameLabelMap(name)

+--------------+------------------------------------------+
| 引数名       | 概要                                     |
+==============+==========================================+
| name         | ムービークリップの名前                   |
+--------------+------------------------------------------+

.. code-block:: javascript

    pex.getAPI().getFrameLabelMap("test");

上記例では、testという名前のMC内にあるタイムラインの各ラベル情報をmap形式のオベジェクトで取得します。
ラベル名がキー値で、その値はラベルが設定されているフレーム番号(1からはじまる)です。

戻り値オブジェクトの例) {'labelA':1,'labelB':10}



setVisible
**********

指定したMCのvisible設定を変更します。

..

	setVisible(name, value)

+--------------+------------------------------------------+
| 引数名       | 概要                                     |
+==============+==========================================+
| name         | ムービークリップの名前                   |
+--------------+------------------------------------------+
| value        | visible値(trueもしくはfalse)             |
+--------------+------------------------------------------+

.. code-block:: javascript

    pex.getAPI().setVisible("test", false);

上記例では、testという名前のMCを非表示にします。
変更に成功した場合はtrue、失敗した場合はfalseを返します。


setPosition
***********

指定したムービークリップの位置を設定します。

..

	setPosition(name, x, y)


+--------------+------------------------------------------+
| 引数名       | 概要                                     |
+==============+==========================================+
| name         | ムービークリップの名前                   |
+--------------+------------------------------------------+
| x            | _x プロパティの値                        |
+--------------+------------------------------------------+
| y            | _y プロパティの値                        |
+--------------+------------------------------------------+


redraw
******

強制的に再描画を行います。
本来であれば、API でムービークリップの位置や visible 設定などを変更しても次のフレームの処理が行われるまで描画結果には反映されませんが、redraw API を使用することで即時反映させることができます。

..

	redraw()


addEventListener
****************

イベントリスナを追加します。同じイベントに複数のリスナを追加した場合は先に追加したリスナから順に呼ばれます。

..

	addEventListener(eventName, listener, name)


+--------------+----------------------------------------------------+
| 引数名       | 概要                                               |
+==============+====================================================+
| eventName    | 対象とするイベントの名前 (case-insensitive)        |
+--------------+----------------------------------------------------+
| listener     | イベントが発生する度に呼び出されるコールバック関数 |
+--------------+----------------------------------------------------+
| name         | ムービークリップの名前                             |
+--------------+----------------------------------------------------+


イベント
--------

enterframe
^^^^^^^^^^

ActionScript 2.0 の MovieClip.onEnterFrame handler 相当のイベントです。コールバック関数の第1引数には API インスタンス、第2引数にはムービークリップ名、第3引数にはムービークリップの _currentframe の値が渡ります。

.. code-block:: javascript

    pex.getAPI().addEventListener("enterframe", function(api, name, currentframe) {
        console.log(name + ": " + currentframe);
    }, "test");

上記例では test という名前のムービークリップにイベントを追加しています。なお、ムービークリップ名を省略した場合は root ムービークリップに対してイベントを追加します。



movieclipcreate
^^^^^^^^^^^^^^^

ムービークリップが作成される度に発生するイベントで、ActionScript の処理が全て終わった後に作成されたムービークリップの数だけコールバック関数が呼ばれます。コールバック関数の第1引数には API インスタンス、第2引数には作成されたムービークリップ名が渡ります。
なお、name 引数にムービークリップ名を指定する必要はありません。

.. code-block:: javascript

    pex.getAPI().addEventListener("movieclipcreate", function(api, name) {
        console.log(name + " is created");
    });



removeEventListener
*******************

..

	removeEventListener(eventName, listener, name)


+--------------+----------------------------------------------------+
| 引数名       | 概要                                               |
+==============+====================================================+
| eventName    | 対象とするイベントの名前                           |
+--------------+----------------------------------------------------+
| listener     | イベントが発生する際に呼び出されるコールバック関数 |
+--------------+----------------------------------------------------+
| name         | ムービークリップの名前                             |
+--------------+----------------------------------------------------+



その他 API
==========

getFPS
******

現在設定されている FPS を取得します。

..

	getFPS()


setFPS
******

動的に FPS を設定します。

..

	setFPS(fps)


+--------------+--------------------------------------------------+
| 引数名       | 概要                                             |
+==============+==================================================+
| fps          | 設定する FPS の値                                |
+--------------+--------------------------------------------------+


getFrameSkipRatio
*****************

現在設定されているフレームスキップの割合を取得します。

..

	getFrameSkipRatio()


setFrameSkipRatio
*****************

定期的にフレームスキップする割合を0から1の間で設定します。例えば0.5の場合は2フレームごとに1フレームスキップし、0.667の場合は3フレームごとに2フレームスキップします。デフォルトは0で、フレームが定期的にスキップすることはありません。
なお、フレームスキップ機能はこの値に関係なく働きます。

..

	setFrameSkipRatio(ratio)


+--------------+--------------------------------------------------+
| 引数名       | 概要                                             |
+==============+==================================================+
| ratio        | 設定するフレームスキップの割合                   |
+--------------+--------------------------------------------------+


