2021.11.24

1. 시작하기
    - Firestore Database
        1. 들어가면 몽고디비랑 비슷하다
        2. 컬렉션
        <!--table ---- collection
        - row ---------- document
        그리고 몽고디비와 같이 정렬이없고, 순서도 따로 없다--> 

    -   Hosting 설정하기 
        1. npm install -g firebase-tools
        2. firebase login
        3. firebase init(설정)
            - firestore 설정 
              1. Hosting: Configure files for Firebase Hosting and (optionally) set up GitHub Action deploys
              2. Firestore: Configure security rules and indexes files for Firestore
              3. Storage: Configure a security rules file for Cloud Storage
        4. firebase deploy(업데이트)    <!-- 외부에서 하게 되면 사용이 끝나고 logout을 한다-->     
        5. rules파일에 들어가서 if true로 고치기       
        


2. FireBase 사용법
    - https://firebase.google.com/docs/firestore/quickstart?authuser=0#web-version-8_4


3. 간단한 홈페이지 템블릿으로 구축법
    - https://html5up.net/

    1) 홈페이지 들어가서 템블릿 확인후 다운받기
    2) public 폴더내에 압축풀기
    3) 끝    


4. 
    1. 프로젝트 개요설정에서 npm, cdn 고르기

    2. 데이터 넣기, 보기

        1) 데이터 넣기

            ```

            if (db) {
                db.collection("car").add({

                    name: "s80",
                    price: 2000,
                    company: "Volvo",
                    year: 2018

            }).then((docRef) => {
                console.log("Document written with ID: ", docRef.id);
            }).catch((error) => {
                console.error("Error adding document: ", error);
            });

            db.collection("car").get().then((querySnapshot) => {
                querySnapshot.forEach((doc) => {
                    console.log(`${doc.id} => ${doc.data().name}`);
            });
            });
            } else {
                console.log('db연결 실패!')
            }
            });

            ```

        2) 데이터 보기

            ```      

            function List() {
            var showListp = `
                <tr>
                    <th>순서</th>
                    <th>차종</th>
                    <th>가격</th>
                    <th>제조사</th>
                    <th>연식</th>
                </tr>`;

            if (db) {
            for (var i = 0; i < carList.length; i++) {
                showList += `
                    <tr>
                        <td>${carList[i].objectId}</td>
                        <td>${carList[i].name}</td>
                        <td>${carList[i].price}</td>
                        <td>${carList[i].company}</td>
                        <td>${carList[i].year}</td>
                    </tr>`;
                    }
                $('table#car_list').html(showList);
            } else {
                console.log('db연결 실패!');
                }
            }

            ```

5. underscore들어가서 모듈? 만들기
    - https://underscorejs.org/ 
    <!-- 이번에는 안했음 -->



과제 : 차종으로 검색하는거 만들기**

내일 하는것 : 검색, 수정, 삭제, 사진업로드





2021-11-25
 자바, ajax, react, renox *** 알아서 공부해보기

1. 파일 업로드, 프리뷰, 삭제, 멀티업로드 개별 삭제

    1) html 파일만들면 <head> 태그 부분에 다운로드 받은 jquery경로 붙이기
            - <script src="js/jquery.js"></script>

    2) <body> 부분에 <input>, <div>프리뷰 만들기

    3) 파일 업로드
        - <script>를 <body> 아래에 생성해서 사용함

        - 파일이 1개이상 선택되어 실행되게함 

        - 특징
            - FileReader 객체 : 파일 소스를 읽어 들인다
            - XMLHttpRequest 객체를 이용한 Ajax와 유사하다. 
            - fetch, axios, jquery ajax ...
        
        - 이미지를 모두 읽어 들이면 실행하도록 설정한다 *  

        - 함수이름 만들때는 동사 + 명사로 만듦 보통 on 은 이벤트 핸들러(변수 =  동사, 메소드 = 명사)

        - function = 화살표(=>) 함수다

        ```

        function readImage(input) {
            if (input.files.length !=0 && input.files[0]) {
                const reader = new FileReader();
                reader.onload = function (e) {  // * 특징
                    imgTag = `<img src=${e.target.result} />`;
                    $('div#프리뷰name').append(imgTag);
                }
                render.readAsDataURL(input.files[0]);
            }
        }

        ```

    4) 파일 프리뷰       
        - 서버가 실행된 상태에서 테스트 가능

        - jquery는 이벤트가 튀는 경우가 있다

        ```
        function readImage(input) {
            for (var i= 0; i<input.files.length; i++) {
            var readfileURL = URL.createObjectURL(input.files[0]);

            var imgTag = `<img src = ${readfileURL} />`;
            $('div#preview-image').append(imgTag);
            
            // 이미지를 로딩 후에 메모리에서 해제하는 작업
            $('div#preview-image>img').on('load', function(e) {
                URL.revokeObjectURL($(this).attr('src'));
            })
        }

        ```


    5) 프리뷰 파일 삭제

        - 프리뷰함수에 readImage에 같이 쓰면된다

        ```
        <input type="button" id="deleteBtn" value="삭제" />; // 이거를 이미지 태그에 넣는다

        $('input.deleteBtn').on('click', function (evt) {
            $('div#preview-image').html(""); // 이미지 지우기
            inpput.value = ""; // 인풋된 파일명 지우기
        })

        ```

    6) 프리뷰 파일 다중 업로드시 개별 삭제

        - 파일 업로드할때 FileList를 동적으로 변경 시켜야한다

        - input 태그에 index를 넣어서 활용하면 된다

        - 참고사이트 https://coco-log.tistory.com/161

        ```
        <input data-index${input.files[i].lastmodified} type="button" id="deleteBtn" value="삭제" />; 
        // data-index${input.files[i].lastmodified}를 추가해 넣는다

        삭제 태그아래에 추가로 적는다
        var filleArr = Array.from(input.files);
        newArr = fileArr.filter(function (file) {
            return file.lastmodified ! = evt.target.dataset.index;
        });

        var datTransfer = new DataTransfer();
        for(var i=0;  i<newArr. length; i++) {
            DataTransfer.items.add(newArr[i]);
        }
        input.files = DataTransfer.files;

        if (this){
            var imgItem = $(this).prev()
        })

        ```


2. firebase와 결합하기

    1) 위에서 했던걸 그대로 스크립트를 가지고온다.

    2) 태그들을 정리한다.

    3) 이미지 파일 업로드
        - saveBtn 쪽을 수정한다

        ```

        var newFileName = ""; // 개체선언
        var downloadUrl = ""; // 개체선언

        var timestamp = new Date().getTime();
        var fileData = $('input#imageSelector')[0].files[0];
        var fileDataName = fileData.name;
        // 파일 확장자 추출하기
        var suffix = fileDataName.substr(fileData.name.lastIndexOf("."));
        var newFileName = timestamp + suffix;
        // 파일 저장 경로
        var uploadRef = storage.ref();
        // 서버에 저장 하위 폴더 만들기 - 자식폴더 경로에 올리기
        var fileSavePath = uploadRef.child(newFileName);
        console.log("fileSavePath>>>> ", fileSavePath);
        // 파일을 storage에 저장하기 - put()
        var result = fileSavePath.put(fileData).then(function (snapshot) {
          // 사진 저장 완료 후 처리 부분.
          snapshot.ref.getDownloadURL().then(function (downloadUrl) {
            saveData(name, price, company, year, newFileName, downloadUrl);
          }).catch(function (err) {
            console.error(">>>>> Error get downloadUrl : ", err);
          });
        }).catch(function (err2) {
          console.error(">>>>> Error put file : ", err2);
        });

        ```

    4) 삭제 구현
        - loadData쪽을 수정한다

        ```

        showList(); //showList를 호출한다
          $('input.delRowBtn').on('click', function (evt) {
            
            // 이미지 삭제, 데이터 삭제
            var objectId = evt.target.dataset.index;
            var imgUrl = evt.target.dataset.imgurl;

            console.log(imgUrl); // 선언한 imgURL이 잘오나 확인한다

            db.collection('cars').doc(objectId).delete().then(function () {
              var delImgRef = storage.ref().child(imgUrl).delete().then(function () {
                alert('이미지와 데이터 삭제 완료');
                loadData();
              }).catch(function (err2) {
                console.log('이미지 삭제 실패 >>> ', err2);
              }) // 이미지 삭제가 안되면 보통 index쪽에서 호출이 안되는  문제가 있다
            }).catch(function (err) {
              console.log('데이타 삭제 실패 >>> ', err);
            });

        ```
        


11.25과제 수정, 상세보기