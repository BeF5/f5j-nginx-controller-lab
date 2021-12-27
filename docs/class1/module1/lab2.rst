Lab 2 - NGINX Controller の冗長化
############################################

このラボのゴールはNGINX Controller clusterの3つ目のメンバーとしてホストを追加することです

本番環境では、我々のサービスに対し冗長性をもたせることが一般的です。複数のインスタンスを水平スケール・動作の管理を行うNGINX Controllerを用いた場合、単一の事象が様々な事象の引き金となることがあります。冗長構成の実装はコンプライアンスやルールの要素としても求められることがあります。この機能により複数のコントローラを用いて安定した環境を実現します

.. IMPORTANT::
    想定時間: 15分

.. NOTE::
    このLabの手順はラボを実施する方がWindows jumphost -- ``jumphost-1`` から操作する手順を示しています。
    接続方法についてはこちらを参照ください。 :ref:`overview` 

追加するNGINX Controllerのノードを作成する
------------------------------------------

#. jumphostのChromeで開かれているNGINX Controllerの管理画面を操作します。証明書エラーが表示されている場合には適切に操作をして画面を開いてください

   .. image:: ../media/ControllerLogin.png
      :width: 400

#. もし開かれていない場合、Chromeブラウザを開いてください

#. BookmarkからNGINX Controller UIにアクセスしてください

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

#. 画面左上のナビゲーションバーを開き、ドロップダウンリストから **Platform** を選択してください

   .. image:: ../media/Tile-Platform.png
      :width: 200

#. **Cluster** を開いてください

   .. image:: ./media/M1L2ClusterTile.png
      :width: 800

#. 現在の "Cluster Configuration" を確認してください

   .. image:: ./media/M1L2ClusterConfig.png
      :width: 800

.. NOTE::
     "Cluster Configuration" の項目は、クラスタを構成するNGINX Controllerインスタンスを示します。
     FQDNはAPI Gateway podに割り当てる証明書で利用するcommon nameに該当します
     例: APIエンドポイントやGUIの接続先として公開するサービスの名称

.. IMPORTANT::
    "load balancer"設定は今後リリースされるNGINX Controllerにて設定可能となる予定です
    追加の情報はラボの :ref:`Reference` を参照してください

.. NOTE::
    "Nodes"として現在2つのNGINX Controller インスタンスが表示されています
    ( "controller-1" および "controller-2" に該当するノード)
