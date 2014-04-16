===============================
HTML5 構成要素として使う
===============================

Pex は複数の SWF を生成し、相互に通信させ、あるいは JavaScript、CSS と組み合わせるなど HTML5 の機能を補間強化する目的で開発されています。

上記 HTML5 ライブラリとして活用する際の TIPS やノウハウを記載します。


Basic Knowledge
************************************

一度取得した SWF は使いまわすことが出来、複数インスタンスの生成に最適化されています。

例えば RPG の敵キャラ、落ち物パズルゲームのユニットなど、ひとつの SWF が複数登場するといった場面では一度取得した SWF を再利用して使うため、XHR や SWF のバイナリパースコストはインスタンス数に比例せず常に 1 です。

.. code-block:: javascript

	// 初回なので XHR による unit.swf 取得、パースが行われる
	var unit_a = new Pex('unit.swf', 'element-a');
	
	// 取得済みの unit.swf が再利用される
	var unit_b = new Pex('unit.swf', 'element-b');
	var unit_c = new Pex('unit.swf', 'element-c');
	var unit_d = new Pex('unit.swf', 'element-d');


第二引数に canvas や DOM 要素を直接渡すことができます。
大量の canvas 要素を扱う場合など、DOM ツリーの変更を最小に抑えるため、予め HTML 側に用意しておくのが良いでしょう。

.. code-block:: html

	<div id="monsters">
		<canvas id="slime-a"></canvas>
		<canvas id="slime-b"></canvas>
		<canvas id="slime-c"></canvas>
	</div>
	<script>
	  var option = {"width":480};
  
	  // 既に DOM ツリーに存在する要素を直接渡すため DOM 構造が変化しない
	  var a = new Pex('slime.swf', document.getElementById('slime-a'), option);
	  var b = new Pex('slime.swf', document.getElementById('slime-b'), option);
	  var c = new Pex('slime.swf', document.getElementById('slime-c'), option);
	</script>

また canvas 要素を渡した場合、必ず option.width の指定が必要です。
debug:true を指定すると getComputedStyle と option.width の値が比較され、値が異なる場合はコンソールに警告を流します。開発等にご利用ください。

