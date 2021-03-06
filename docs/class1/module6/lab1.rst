Lab 1 - GUI で Snippets を設定する
######################################################

このラボのゴールは、v3.22.0 から追加された機能 "Snippets" の設定と動作の確認です。

.. IMPORTANT::
    想定時間: 10分

.. NOTE::
    このラボの手順はラボを実施する方がWindows jumphost -- ``jumphost-1`` から操作する手順を示しています。
    接続方法についてはこちらを参照ください。 :ref:`overview` 


Snippets について
----

Snippets はNGINX Controller ADCのGUI/APIで、通常のNGINX Configと同様に記述した設定を **そのまま** 対象のNGINX Plusに追加・反映する機能です。

Snippets は5種類の設定項目があり、それぞれの設定に応じて **Gateway** や **Component** のメニューで設定を追加します。

   +------------------------+--------------------------------------------+-----------+
   |      Snippet           |    内容                                    | Endpoint  |
   +========================+============================================+===========+
   | httpSnippet            | 設定をHTTP Blockに追加します               | Gateway   |
   +------------------------+--------------------------------------------+-----------+
   | mainSnippet            | 設定をMain Blockに追加します               | Gateway   |
   +------------------------+--------------------------------------------+-----------+
   | streamSnippet          | 設定をStream Blockに追加します             | Gateway   |
   +------------------------+--------------------------------------------+-----------+
   | uriSnippets            | 設定を対象GatewayのServer Blockに追加します| Gateway   |
   +------------------------+--------------------------------------------+-----------+
   | uriSnippets            | | 設定を対象ComponentのServer Block        | Component |
   |                        | | またはLocation Blockに追加します         |           |
   +------------------------+--------------------------------------------+-----------+
   | workloadGroupSnippets  | 設定をUpstream Blockに追加します           | Component |
   +------------------------+--------------------------------------------+-----------+

.. NOTE::
    URI Snippets は TCP/UDP のコンポーネントでは動作しない場合があります

その他 Snippets の詳細については `About Snippets <https://docs.nginx.com/nginx-controller/app-delivery/about-snippets/>`__ をご確認ください

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

   .. image:: ./media/M6L1gateways.png
      :width: 200

#. **echoappgw** を開いてください

   .. image:: ./media/M6L1echoappgw.png
       :width: 600

#. "Edit Config" をクリックし、設定画面に移動します

   .. image:: ./media/M6L1echoappgw-Edit.png
       :width: 600

#. "Additional" をクリックします。Config Snippets まで画面をスクロールし、Gateway で対応しているSnippetsに設定を追加します

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

     - .. image:: ./media/M6L1echoappgw-GatewaySnippet.png
          :width: 600


#. **Submit** をクリックし、操作を完了させてください

   .. image:: ./media/M6L1Submit.png
      :width: 300

App Componentを開き、Snippetを追加します
----

#. "Apps" を選択してください

   .. image:: ../media/Services-Apps.png
      :width: 200

#. **echoapp** を開いてください

   .. image:: ./media/M6L1echoapp.png
       :width: 600

#. **echoappcomponent** を開いてください

   .. image:: ./media/M6L1echoappcomponent.png
       :width: 600

#. "Edit Config" を選択してください

   .. image:: ./media/M6L1echoappcomponent-EditConfig.png
      :width: 400

#. "Snippets" をクリックしてください。 "URI Snippets" 、 "Workload Group Snippets" の欄があります。各設定を追加するため、 **Add URI Snippet** 、 **Add Workload Group Snippet** をクリックしてください

   .. image:: ./media/M6L1echoappcomponent-Snippets.png
       :width: 600

#. Component で対応しているSnippetsに設定を追加します。以下の内容を参考に設定を追加してください

   +------------------------------+----------------------------------------------------------------+
   |        Field                 |      Value                                                     |
   +==============================+================================================================+
   |  URI Snippet                 | ``allow 192.168.4.1/32;``                                      |
   +------------------------------+----------------------------------------------------------------+
   |  Applicable URIs             | ``/``                                                          |
   +------------------------------+----------------------------------------------------------------+
   |  Workload Group Snippet      | ``sticky cookie echo_cookie expires=3h domain=.$host path=/;`` |
   +------------------------------+----------------------------------------------------------------+
   |  Applicable Workload Groups  | ``Echo Backend`` (自動的に Select Allもチェックされます)       |
   +------------------------------+----------------------------------------------------------------+

   - 設定追加画面

     - .. image:: ./media/M6L1echoappcomponent-URISnippets.png
          :width: 600
   
     - .. image:: ./media/M6L1echoappcomponent-WLSnippets.png
          :width: 600


#. **Submit** をクリックし、操作を完了させてください

   .. image:: ./media/M6L1Submit.png
      :width: 300

CLIより、Snippet で追加した内容を確認します
----

#. "nginxplus-3" インスタンスにログインしてください。"PuTTY" を開き、保存済みのホストより **nginxplus-3** を選択し、**Open** をクリックしてください

   .. image:: ./media/M6L1Putty.png
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
    - URI Snippet で指定した内容は、28行目、29行目の通り Applicable URIs で指定した server_name と listen port に該当する Server Block で有効になっており、30行目で許可アドレスが設定されています

- Component Snippet
    - URI Snippet で指定した内容は、Componentの対象となる、location / 内となる 25行目、32行目に設定されています
    - Workload Group Snippet で指定した内容は、Echo Backend 内のセッションパーシステンスとして 6行目に設定されています




おめでとうございます！！ NGINX Controller Lab はこれで完了です。
----