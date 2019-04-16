# MeshStreaming_via_DB  

[![](https://img.youtube.com/vi/jyTUU44PlE8/0.jpg)](https://www.youtube.com/watch?v=jyTUU44PlE8)  

### Grasshopper から PostgreSQL に接続する  

Grasshopper から データベースにアクセスするためのアドオンとして、 Slingshot! というものがあるっぽい。  
が、落としていろいろ試すも、PostgreSQL に接続できなかったので、スクリプトコンポーネントで頑張るに方針転換した。

結果として、GH_CPython と、psycopg2 でうまくいった。  


### index  
- 環境  
- psycopg2  
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

違うかも。  
好きな Python インタプリタのパスを通せば良い。  
anaconda とか。3系も可。  



---  


### psycopg2  

普通に Python 環境下で書くのと同じ  
conn.commit() が必要  


```python
import psycopg2

def get_connection():
    connect_str = "dbname='postgres' user='postgres' host='localhost' password='postgres'"
    return psycopg2.connect(connect_str)

conn = get_connection()
cur = conn.cursor()

if TF == True:


    # SELECT  
    sql_s = "SELECT * FROM " + table_name + ";"
    # debug = sql_s
    cur.execute(sql_s)
    rows = cur.fetchall()
    _output = rows


    # DELETE  
    sql_del = "DELETE FROM " + table_name + ";"
    # debug = sql_del
    cur.execute(sql_del)
    conn.commit() # 大事！！


    # INSERT  
    for i in range(len(id)):
        sql_insert = "INSERT INTO " + table_name + " (mesh_id, pt0, pt1, pt2) " + " VALUES (" +str(i)+ "," + str(pt0[i]) + "," + str(pt1[i]) + "," + str(pt2[i]) + " );"
        # debug = sql_insert
        cur.execute(sql_insert)
        conn.commit() # 大事！！


cur.close()
conn.close()


```


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

##### 現状  

2つのテーブルに書き込んで後でくっつける  
gh の DeconstructionMesh と DeconstructionFace からそのまま  
- Vertex テーブル : Vertex の頂点情報のリスト  
- Mesh テーブル   : Mesh の三点のリスト  
この2つのテーブルに情報を書き込んで、受け取る側で整える  
（もしかしたら、データベース上できちんとくっつけて出力？するのがいい？）  

Vertex テーブル  

|ver_id|ver_x|ver_y|ver_z|  
|:---|:---|:---|:---|  
|1|2.342|4.654|9.384|  
|2|6.323|7.511|1.552|  
|.|.|.|.|  
|m|3.924|2.453|6.454|  

(4 x m)  

Mesh テーブル  

|mesh_id|pt0|pt1|pt2|  
|:---|:---|:---|:---|  
|1|232|64|84|  
|2|63|11|5|  
|.|.|.|.|  
|n|34|53|654|  

(4 x n)  

仮に、6000 の頂点 + 13000 のメッシュ面であるとすると、データベースに書き込む量は、76000 個（これで5-7秒）  



##### 別案  

1つのテーブルに書き込む  
テーブルがすごくシンプル  

|mesh_id|pt0_x|pt0_y|pt0_z|pt1_x|pt1_y|pt1_z|pt2_x|pt2_y|pt2_z|  
|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|  
|1|2.342|4.654|9.384|6.323|7.511|1.552|3.924|2.453|6.454|  
|2|6.323|7.511|2.342|4.654|9.384|9.384|6.323|7.511|9.384|  
|.|.|.|.|.|.|.|.|.|.|
|p|2.342|4.654|9.384|7.511|2.342|7.511|2.342|7.511|2.342|  

(p x 10)  

仮に、13000 のメッシュ面であるとすると、データベースに書き込む量は、130000 個  


---  
