Lab 1 - GUI で Snippets を設定する
######################################################

このラボのゴールは、現在NGINX Controllerで利用可能な "Caching" 機能を確認します。
v3.22.0 から追加された機能でGUI/APIを使って設定を追加することが可能です。


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