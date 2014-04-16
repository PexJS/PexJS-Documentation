======================================
Quick Start
======================================

Pex を利用するには2つのファイルが必要です。

- pex.min.js
- 動作させる SWF ファイル (example.swf)


次のように初期化を行います。

.. code-block:: javascript

	var pex = new Pex(src, container, option);


それぞれの引数の意味は次のとおりです。

+--------------+------------------------------------------+
| 引数名       | 概要                                     |
+==============+==========================================+
| src          | SWF ファイルへの相対パス、または絶対パス |
+--------------+------------------------------------------+
| container    | SWF の描画先となる DOM の ID 属性値      |
+--------------+------------------------------------------+
| option       | Pex のオプション                         |
+--------------+------------------------------------------+


次のコードは Pex 上で SWF を動かすための最小限のサンプルです。

.. code-block:: html

	<html>
	    <head>
	        <title>Pex Example</title>
	    </head>
	    <body>
	        <div id="container"></div>

	        <!-- end of body -->
	        <script type="text/javascript" src="pex.min.js"></script>
	        <script type="text/javascript">
	        var option = {};
	        var pex = new Pex("./example.swf", "container", option);
	        </script>
	    </body>
	</html>

