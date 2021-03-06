# Docker Command Notes

筆記 docker 指令速查表，方便操作上查詢

參考:
 - [《Docker —— 從入門到實踐­》正體中文版](https://www.gitbook.com/book/philipzheng/docker_practice/details)
 - [yeasy docker practice 2017年 正體中文譯本 0.8.1](https://www.gitbook.com/book/wild0522/yeasy_dp/details)

## images

* 映像檔：
	* 取得: pull \<name\>:\<tag\> 
	* 顯示: images
	* 移除: rmi \<name\>:\<tag\> | \<PID\> 
	* 修改已有映像檔： commit -m 'commit message' -a 'user message' \<container\_PID\> \<container\_name:container_tag\>

```
$ docker pull ubuntu:12.04
$ docker images
$ docker rmi test:v0.01
$ docker commit -m 'new images' -a 'docker newbee' test:v0.0.1 test:v0.0.2
```

## Dockerfile

* dockerfile:
	* 建立: build [OPTIONS] image\_name:image\_tag .
		* -t: 指定新的映像檔訊息 
		* --build-arg: 自帶變數，dockerfile內可用 ARG 取得
		* . : dockerfile 檔前目錄，也可用具體的dockerfile目錄
	* file命令：
		* RUN: 建立中執行的指令	 
		* CMD: 容器啟動後接著要執行的程序
			* CMD ['executable', 'param1', 'param2']
			* CMD command param1 param2
			* CMD ['param1', 'param2']
		* ENTRYPOINT: 建立容器後要執行或運行的檔案
		* WORKDIR: 切換工作目錄
		* ARG: 建置變數
		* ENV: 環境變數
		* LABEL: 定義的標籤可以使用在 docker 指令中作為過濾用途，在 instance 內是無法取得。
		* ADD <src> <dest>: 本地目錄複製到容器目錄
		* COPY <src> <dest>: 複製本地端到容器內
		* EXPOSE prot: 向對外部開放 port
		* ${variable:-word} 代表，如果variable有給值，則以variable設定的文字為主，如未設定，則以word字串為主。
		* ${variable:+word} 代表，如果variable有給值，則值為word；如果variable未給值，則最後結果為空字串(empty)。	


dockerfile範例

```
# This is a commit
FROM ubuntu:14.04
MAINTAINER Docker Newbee <xx@xxxx.com>
RUN apt-get -qq update
...more commad

```

## container

* 容器： 
	* 顯示執行中： ps [OPTIONS]  
		-a: 所有狀態
	* 執行： run [OPTIONS] 
		* -t: 分配一個虛擬終端並綁定到標準輸入上
		* -i: 容器的標準輸入保持打開
		* -d: 以守護狀態執行
		* -v: 建立容器資料卷或指定一個本機目錄到容器中		* -c: 進入到容器後執行commad
		* -p: 容器port對應到本機port
			* 映射所有遠端位址: hostPort:containerPost
			* 映射到指定位址: ip:host:containerPost 
			* 映射定指定位址任意連接port: ip::containerPort
			* 可以多次使用來綁定多個port
		* -e:
		* --name: 容器名子
		* --volumes-from: 指定一個已有的容器來掛載資料
		* -h: 容器主機名
		* -rm: 終止後立刻刪除，不可與 -d 同時使用
		* --link: 容器之間進行互動
		* 
			* name:alias 

	* 啟動: start \<PID | Name\>
	* 重啟: restart \<PID | Name\>
	* 停止: stop \<PID | Name\>
	* 進入: exec -ti \<PID | Name\> bash | attach \<PID | Name\> | nsenter \<PID | Name\>
	* 匯出本地容器: docker export \<container\_PID\> > export\_file\_name.tar
	* 映像檔: import \<file | url\> \<image\_name\>
	* 刪除： rm \<PID | Name\> [-f | -v]
		* -f：執行中也可移除 
		* -v:刪除關聯容器

```
$ docker ps -a
$ docker run -tid -p 8080:8080 -v home/project:/opt/project --name my-app ubuntu:14.04 /bin/sh -c "command" --link db:db
$ docker run -rm -it -v ~/.bash_history:/.bash_history ubuntu /bin/bash
$ docker run --volumes-from my-app --name new_container training/postgres
$ docker start my-app
$ docker restart my-app
$ docker exec -ti my-app bash
$ docker attach my-app
$ docker stop my-app
$ docker rm -f my-app
```

## Repository

* repository	
	* 登入: login
	* 搜尋: search \<key word\>
	* 推送遠端: push

	
```
$ docker login
$ docer search centos
$ docker push
```

* 其他：
	* 輸出訊息: logs [OPTIONS] \<PID | containr_name\>
		* -f 
	* 查看當前映射的連結port: port \<containr_name\> <port>
	* 獲取所有變數: inspect

```
$ docker logs my_container_name
$ docker inspect
```


