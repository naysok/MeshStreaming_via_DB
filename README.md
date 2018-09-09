# MeshStreaming_via_DB  


### Grasshopper から PostgreSQL に接続す  

Grasshopper から データベースにアクセスするためのアドオンとして、 Slingshot! というものがあるっぽい。  
が、落としていろいろ試すも、PostgreSQL に接続できなかったので、スクリプトコンポーネントで頑張るに方針転換した。

結果として、GH_CPython と、psycopg2 でうまくいった。  


##### GH_CPython  
[https://www.food4rhino.com/app/ghcpython](https://www.food4rhino.com/app/ghcpython)  

※ユーザーオブジェクトにビルドできる？便利そう  
（[https://github.com/MahmoudAbdelRahman/GH_CPython/wiki/02--First-GH_CPython-plugin](https://github.com/MahmoudAbdelRahman/GH_CPython/wiki/02--First-GH_CPython-plugin)）


##### psycopg2（win版）  
Release Files から落とす  
[http://www.stickpeople.com/projects/python/win-psycopg/](http://www.stickpeople.com/projects/python/win-psycopg/)  


---  


### 環境  

Rhinoceros 5 + Grasshopper 0.9  


---  


### Slingshot!  

とりあえず、最初にずっと触っていた。  
gitbucket にソースがあるので見に行くと、Open Database Connectivity (ODBC) で接続しているっぽいのが確認できた。  
さらに、フォーラムを探ると、現状のバージョン以前のバージョンでは動いていたようで、ソースを探すと、　slingshot / Grasshopper / TPGNM.SlingshotDB / SlingshotDB / LEGACY の中になんかあるので、自分で修正するとか等少し考えたものの、  
Visual Studio 画手元にないので断念。  

[https://www.grasshopper3d.com/group/slingshot](https://www.grasshopper3d.com/group/slingshot)  
[http://wiki.theprovingground.org/slingshot](http://wiki.theprovingground.org/slingshot)  
[https://bitbucket.org/archinate/slingshot](https://bitbucket.org/archinate/slingshot)




---  

PostgreSQL  

```SQL
CREATE TABLE Vertex_180824 (vertex_id SERIAL PRIMARY KEY, ver_x NUMERIC, ver_y NUMERIC, ver_z NUMERIC);

```
