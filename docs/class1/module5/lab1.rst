Lab 1 - ADC で Caching を有効にする
######################################################

このラボのゴールは、現在NGINX Controllerで利用可能な "Caching" 機能を確認します。
v3.22.0 から追加された機能でGUI/APIを使って設定を追加することが可能です。

.. IMPORTANT::
    想定時間: 10分

.. NOTE::
    このLabの手順はラボを実施する方がWindows jumphost -- ``jumphost-1`` から操作する手順を示しています。
    接続方法についてはこちらを参照ください。 :ref:`overview` 


NGINX Plus にキャッシュを保存するディレクトリを作成する
----

#. "nginxplus-3" インスタンスにログインしてください。"PuTTY" を開き、保存済みのホストより **nginxplus-4** を選択し、**Open** をクリックしてください

   .. image:: ../module1/media/L3Putty.png
      :width: 400

   .. IMPORTANT::
      もし、Puttyがサーバのホスト鍵に関する警告を示した場合、接続のため **Yes** をクリックしてください
      これは、ラボ環境の各ホストでユニークなhost keyを生成するため生じるものです

#. キャッシュで利用するディレクトリを作成してください

   .. code-block:: bash
   
     $ mkdir -p /tmp/cache/store1   

App Componentを開く
-------------------------

#. Chromeを開く

#. ブックマークよりNGINX Controller のGUIにアクセス

   .. image:: ../media/ControllerBookmark.png
      :width: 600

#. NGINX Controller のadmin accountである、``Peter Parker`` でログインしてください

   +-------------------------+-----------------+
   |      Username           |    Password     |
   +=========================+=================+
   | peter@acmefinancial.net | ``Peter123!@#`` |
   +-------------------------+-----------------+

   .. image:: ../media/ControllerLogin-Peter.png
      :width: 400

#. **Services** を開いてください

   .. image:: ../media/Tile-Services.png
      :width: 200

#. "Apps" を選択してください

   .. image:: ../media/Services-Apps.png
      :width: 200

#. "Trading Application (CAS)" app を開いてください

    .. image:: ./media/TradingMainCASApp.png
        :width: 600

#. "Trading Main Component" を選択し、設定を変更してください

    .. image:: ./media/TradingMainCASComponent.png
        :width: 600

Caching を設定する
----

#. "Enable Caching" を有効にし、パラメータを入力します。

   .. image:: ./media/M5L1cache.png
      :width: 200

#. 以下の通り項目を入力してください

   +-------------------------+------------------------+
   |        Field            |      Value             |
   +=========================+========================+
   |  Key                    | ``request_uri``        |
   +-------------------------+------------------------+
   |  Criteria Type          | ``PERCENTAGE``         |
   +-------------------------+------------------------+

   .. image:: ./media/M5L1cache2.png
      :width: 200

#. そのまま画面を下にスクロールし、DISK STOREの内容を以下の通り項目を入力してください

   +-------------------------+------------------------+
   |        Field            |   Value                |
   +=========================+========================+
   |  Path                   | ``/tmp/cache/store1``  |
   +-------------------------+------------------------+
   |  Max Size               | ``10m``                |
   +-------------------------+------------------------+
   |  Min Free               | ``10k``                |
   +-------------------------+------------------------+
   |  In Memory Store Size   | ``5m``                 |
   +-------------------------+------------------------+
   |  Is Default             | ``TRUE``               |
   +-------------------------+------------------------+

   .. image:: ./media/M5L1cache3.png
      :width: 200

#. 左のメニューから ``Programmability`` を開きます。 ``Response Header Modification`` に以下の通り追加します

   +-------------------------+----------------------------+
   |        Field            |   Value                    |
   +=========================+============================+
   |  Action                 | ``ADD``                    |
   +-------------------------+----------------------------+
   |  Header Name            | ``X-Cache-Status``         |
   +-------------------------+----------------------------+
   |  Header Value           | ``$upstream_cache_status`` |
   +-------------------------+----------------------------+

   .. image:: ./media/M5L1cache4.png
      :width: 200

#. 左のメニューから ``Snippets`` を開きます。 ``URL Snippets`` に以下の通り追加します

   .. code-block:: bash
   
     proxy_cache_valid any 1m;
     proxy_ignore_headers Set-Cookie;

   .. image:: ./media/M5L1cache5.png
      :width: 200

#. 画面右上の ``Submit`` をクリックしてください。設定が完了すると以下のようにフォルダが生成されます

   .. image:: ./media/M5L1cache6.png
      :width: 200

   .. code-block:: bash
   
     $ sudo ls -l /tmp/cache/store1/*
     /tmp/cache/store1/app_centric_retail-development|trading|main|:
     total 0

動作を確認する
----

#. Chromeブラウザを開き、 ``Secret Tab (New Incognito Window)`` を開いてください。

   .. image:: ./media/M5L1chrome.png
      :width: 200

#. ブラウザ上で右クリックメニューを開き ``開発者モード(Inspect)`` を開き、 ``Network`` タブに移動してください。

   .. image:: ./media/M5L1chrome2.png
      :width: 200

   .. image:: ./media/M5L1chrome3.png
      :width: 200

#. | キャッシュを生成するため、 ``http://trading.acmefinancial.net/`` へアクセスしてください。
   | 接続の結果から、キャッシュが生成されたか Response Header の情報から確認します。
   | ``section-1-bg.jpg`` を選択し、 ``Response Headers`` の ``X-Cache-Status`` の内容を確認してください

   .. image:: ./media/M5L1cacherequest1.png
      :width: 200

#. | 一旦 ``Secret Tab`` を閉じ、上記手順を参考に再度 ``Secret Tab`` で ``http://trading.acmefinancial.net/`` へアクセスしてください。
   | ``section-1-bg.jpg`` を選択し、 ``Response Headers`` の ``X-Cache-Status`` の内容を確認してください

   .. image:: ./media/M5L1cacherequest2.png
      :width: 200
