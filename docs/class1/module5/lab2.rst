Lab 2 - GUI で Caching のステータスを確認する
######################################################

このラボのゴールは、v3.22.0 から追加された機能 "Caching" のステータスの確認です。

Cachingのステータスについて、 **App** や **Component** の単位で状況を確認いただけます。
この項目では参考に、 **Component** で状態を確認した結果を表示します。

.. IMPORTANT::
    想定時間: 5分

.. NOTE::
    このラボの手順はラボを実施する方がWindows jumphost -- ``jumphost-1`` から操作する手順を示しています。
    接続方法についてはこちらを参照ください。 :ref:`overview` 


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

#. "Trading Main Component" を開いてください

    .. image:: ./media/M5L2TradingMainCASComponent.png
        :width: 600


GUIで状態を確認する
----

Lab1で確認した内容を参考に、ステータスをご確認ください

#. | ``Overview`` をクリックし、 ``Caching Metrics`` のタブを開きます
   | 各項目が表示されておりますので、適宜内容をご確認ください。
   | 右上の項目から、対象となる時間等選択することが可能です。

    .. image:: ./media/M5L2CacheMenu.png
        :width: 600



#. Cache Size: Cache のサイズ

    .. image:: ./media/M5L2CacheStatus1.png
        :width: 600

#. Cache Hit Responses: Cacheサーバが有効なCacheとして応答した数

    .. image:: ./media/M5L2CacheStatus2.png
        :width: 600

#. Cache Miss Responses: Cacheに該当するデータがなく、Originサーバからデータが取得された数。(このMissの後、キャッシュされる場合があります)

    .. image:: ./media/M5L2CacheStatus3.png
        :width: 600

#. Cache Stale Responses: Origina Serverが正しく応答せずStaleとなった数。proxy_cache_use_stale を設定した場合に有効となります

    .. image:: ./media/M5L2CacheStatus4.png
        :width: 600

