# MeshStreaming_via_DB  


### Grasshopper から PostgreSQL に接続する  

Grasshopper から データベースにアクセスするためのアドオンとして、 Slingshot! というものがあるっぽい。  
が、落としていろいろ試すも、PostgreSQL に接続できなかったので、スクリプトコンポーネントで頑張るに方針転換した。

結果として、GH_CPython と、psycopg2 でうまくいった。  


### index  
- 環境  
- Slingshot  
- テーブル構成  


---  

---  


### 環境  

Rhinoceros 5 + Grasshopper 0.9  
GH_CPython v0.1-alpha  
psycopg2 win版  

##### GH_CPython  
[https://www.food4rhino.com/app/ghcpython](https://www.food4rhino.com/app/ghcpython)  

※ユーザーオブジェクトにビルドできる？便利そう  
（[https://github.com/MahmoudAbdelRahman/GH_CPython/wiki/02--First-GH_CPython-plugin](https://github.com/MahmoudAbdelRahman/GH_CPython/wiki/02--First-GH_CPython-plugin)）


##### psycopg2（win版）  
Release Files から落とす  
[http://www.stickpeople.com/projects/python/win-psycopg/](http://www.stickpeople.com/projects/python/win-psycopg/)  

ghCPython で、汎用の Python ライブラリ（モジュール？）が~~うまく使えない~~使えることは使える。  
置く場所はここ  
C:\Users\xxx\AppData\Roaming\McNeel\Rhinoceros\5.0\Plug-ins\IronPython (xxx)\settings\lib  



---  


### Slingshot!  

とりあえず、最初にずっと触っていた。  
gitbucket にソースがあるので見に行くと、Open Database Connectivity (ODBC) で接続しているっぽいのが確認できた。  
さらに、フォーラムを探ると、現状のバージョン以前のバージョンでは動いていたようで、ソースを探すと、　slingshot/Grasshopper/TPGNM.SlingshotDB/SlingshotDB/LEGACY の中になんかあるので、自分で修正するとか等少し考えたものの、Visual Studio が手元にないので断念。  

[https://www.grasshopper3d.com/group/slingshot](https://www.grasshopper3d.com/group/slingshot)  
[http://wiki.theprovingground.org/slingshot](http://wiki.theprovingground.org/slingshot)  
[https://bitbucket.org/archinate/slingshot](https://bitbucket.org/archinate/slingshot)


---  


### テーブル構成  

現状  
gh の DeconstructionMesh と DeconstructionFace からそのまま  
Vertex : Vertex の頂点情報のリスト  
Mesh   : Mesh の三点のリスト  

Vertex  
|ver_id|ver_x|ver_y|ver_z|  
|:---|:---|:---|:---|  
|table|string|テーブルを表示したい|テーブルを表示したい|  
