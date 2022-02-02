Lab 1 - GUI で Snippets を設定する
######################################################

このラボのゴールは、v3.22.0 から追加された機能 "Snippets" の設定と動作の確認です。


Cachingのステータスについて、 **App** や **Component** の単位で状況を確認いただけます。
この項目では参考に、 **Component** で状態を確認した結果を表示します。

.. IMPORTANT::
    想定時間: 5分

.. NOTE::
    このラボの手順はラボを実施する方がWindows jumphost -- ``jumphost-1`` から操作する手順を示しています。
    接続方法についてはこちらを参照ください。 :ref:`overview` 

GUI を開きます
----

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

Gatewayを開き、Snippetを追加します
----

#. "Gatweay" を選択してください

   .. image:: ../media/Services-Apps.png
      :width: 200

#. **echoappgw** を開いてください

    .. image:: ./media/TradingMainCASApp.png
        :width: 600

#. "Edit Config" をクリックし、設定画面に移動します

#. "Additional" をクリックし、Config Snippets まで画面をスクロールします

#. Gateway で対応しているSnippetsに設定を追加します。以下の内容を参考に設定を追加してください

   - 追加パラメータ

   +-------------------------+--------------------------------------+
   |        Field            |      Value                           |
   +=========================+======================================+
   |  Main Snippet           |  ``worker_rlimit_nofile 2048;``      |
   +-------------------------+--------------------------------------+
   |  HTTP Snippet           |  ``allow 192.168.1.0/24;``           |
   +-------------------------+--------------------------------------+
   |  Stream Snippet         |  ``allow 192.168.2.0/24;``           |
   +-------------------------+--------------------------------------+
   |  URI Snippet            |  ``allow 192.168.3.1/32;``           |
   +-------------------------+--------------------------------------+
   |  Applicable URIs        | ``http://echoapp.net``               |
   +-------------------------+--------------------------------------+

   - 設定追加画面
     - .. image:: ../media/ControllerBookmark.png
          :width: 600
     - .. image:: ../media/ControllerBookmark.png
          :width: 600

#. **Submit** をクリックし、操作を完了させてください


App Componentを開き、Snippetを追加します
----

#. "Apps" を選択してください

   .. image:: ../media/Services-Apps.png
      :width: 200

#. "echoapp" を開いてください

    .. image:: ./media/TradingMainCASApp.png
        :width: 600

#. "echoappcomponent" を開いてください

    .. image:: ./media/M6L1TradingMainCASComponent.png
        :width: 600


#. Component で対応しているSnippetsに設定を追加します。以下の内容を参考に設定を追加してください

   - 追加パラメータ

   +------------------------------+----------------------------------------------------------------+
   |        Field                 |      Value                                                     |
   +==============================+================================================================+
   |  URI Snippet                 | ``allow 192.168.4.1/32;``                                      |
   +------------------------------+----------------------------------------------------------------+
   |  Applicable URIs             | ``/``                                                          |
   +------------------------------+----------------------------------------------------------------+
   |  Workload Group Snippet      | ``sticky cookie echo_cookie expires=3h domain=.$host path=/;`` |
   +------------------------------+----------------------------------------------------------------+
   |  Applicable Workload Groups  | ``Echo Backend`` (自動的に Select Allもチェックされます)         |
   +------------------------------+----------------------------------------------------------------+

   - 設定追加画面
     - .. image:: ../media/ControllerBookmark.png
          :width: 600
     - .. image:: ../media/ControllerBookmark.png
          :width: 600


#. **Submit** をクリックし、操作を完了させてください


CLIより、Snippet で追加した内容を確認します
----

#. "nginxplus-3" インスタンスにログインしてください。"PuTTY" を開き、保存済みのホストより **nginxplus-3** を選択し、**Open** をクリックしてください

   .. image:: ../module1/media/L3Putty.png
      :width: 400

   .. IMPORTANT::
      もし、Puttyがサーバのホスト鍵に関する警告を示した場合、接続のため **Yes** をクリックしてください
      これは、ラボ環境の各ホストでユニークなhost keyを生成するため生じるものです

#. 設定を確認します

.. code-block:: bash
  :linenos:
  :caption: Snippet の反映結果確認
  :emphasize-lines: 38,3,40,28,29,30,25,32,6

  $ egrep 'http {|stream {|server {|listen |server_name |location |Echo Backend|allow |echo_cookie|worker_rlimit_nofile' nginx.conf
  http {
          allow 192.168.1.0/24;
          upstream 'Echo Backend_http_68fc5a3b-b6a2-4b9b-b2cd-fdd119d933e8' {
                  zone 'Echo Backend_http_68fc5a3b-b6a2-4b9b-b2cd-fdd119d933e8' 160k;
                  sticky cookie echo_cookie expires=3h domain=.$host path=/;
          server {
                  server_name _;
                  listen 443 ssl;
                  listen 80;
          server {
                  server_name trading.acmefinancial.net;
                  listen 80 reuseport;
                  location / {
                  location /api {
          server {
                  server_name trading.acmefinancial.net;
                  listen 443 ssl reuseport;
                  location / {
                  location /api {
          server {
                  server_name echoapp.net;
                  listen 443 ssl;
                  location / {
                          allow 192.168.4.1/32;
                          proxy_pass 'http://Echo Backend_http_68fc5a3b-b6a2-4b9b-b2cd-fdd119d933e8';
          server {
                  server_name echoapp.net;
                  listen 80;
                  allow 192.168.3.1/32;
                  location / {
                          allow 192.168.4.1/32;
                          proxy_pass 'http://Echo Backend_http_68fc5a3b-b6a2-4b9b-b2cd-fdd119d933e8';
          server {
                  server_name 127.0.0.1;
                  listen 127.0.0.1:49151;
                  location /api {
  worker_rlimit_nofile 2048;
  stream {
          allow 192.168.2.0/24;
  
順に設定について確認します。

- Gatweay Snippet
    - Main Snippet で指定した内容は、38行目に設定されています
    - HTTP Snippet で指定した内容は、3行目に設定されています
    - Stream Snippet で指定した内容は、40行目に設定されています
    - URI Snippet で指定した内容は、28行目、29行目の通り Applicable URIs で指定した erver_name と listen port に該当する Server Block で有効になっており、30行目で許可アドレスが設定されています

- Component Snippet
    - URI Snippet で指定した内容は、Componentの対象となる、location / 内となる 25行目、32行目に設定されています
    - Workload Group Snippet で指定した内容は、Echo Backend 内のセッションパーシステンスとして 6行目に設定されています




おめでとうございます！！ NGINX Controller Lab はこれで完了です。
----