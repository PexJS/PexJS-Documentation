======================================
Pex Runner
======================================

Pex による SWF 再生テスト補助を行うツールです。


セットアップ
============================

 - コマンド rake -T にてタスク一覧が表示されるよう Rake, Haml (Ruby Haml) をインストールしてください。
 - pex 本体（pex.*.min.js）を pex/ 以下に設置してください。


HTML 生成
============================

 - swf 以下にテスト対象となる swf を配置します。
 - rake コマンドを実行してください。 次のようにオプションを用いて Pex をファイル名で指定できます。
 - output 以下に HTML セットが生成されます。

::

  rake pex=pex.20120712e107.min.js


補助機能
--------------------------------

swf ディレクトリ以下に JavaScript, JSON ファイルを設置して Pex のオプション、操作を指定する事が可能です。


 - global.json: Pex のオプションを指定します。 全 SWF に対して適用されます。
 - swfname.json: Pex のオプションを指定します。 ファイル名一致する SWF にのみ適用されます。
 - global.js: Pex の操作を行う等、追加の JavaScript を記述可能です。全 SWF に適用されます。
 - swfname.js: 追加の JavaScript を記述可能です。ファイル名と一致する SWF にのみ適用されます。


実機確認
============================

実機確認を行う場合、実機からネットワークアクセス可能なサーバにファイル一式をアップロードする必要があります。
rsync タスク、または他の方法でアップロードしてください。

rsync タスクを利用する場合は下記のようにディレクトリ直下の config.json ファイルに rsync コマンドを指定してください。

.. code-block:: json

	{
		"rsync": "example.com:~/path_to_dir"
		（... 略 ...）
	}


レポート機能
============================

リモートサーバに設置された HTML に対しスクリーンショットとブラウザログの記録を自動で行う機能です。
大量の SWF をざっと確認したい場合等に用います。


セットアップ
--------------------------------

 - PhantomJS をインストールしてください
 - ディレクトリ直下の config.json にアクセス先を指定します。 HTTP サーバ上の output/index.html を示す URL を設定してください。

.. code-block:: json

	{
		"report": {
			"target": "http://example.com/path_to_outputdir/index.html"
		}
		（... 略 ...）
	}


利用方法
--------------------------------

::

  rake report

また以下のコマンドにより最新のレポートに含まれるブラウザログから異常のあったものを抽出表示します。

::

  rake report mode=read


その他
============================

フルスクリーン化ユーティリティ
--------------------------------

フルスクリーン化ユーティリティを同梱しております。
以下に利用例を示します。

.. code-block:: json

	{
	  "stopOnStart":true
	}


.. code-block:: javascript

	var sc = new FullScreen;
	pex = new Pex(swf, container, option);
	pex.getAPI().ready = function() {
		sc.display(container);
		pex.getAPI().play();
	};



