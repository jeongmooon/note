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