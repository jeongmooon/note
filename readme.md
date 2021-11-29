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

11.29

- 김범준 강사님 git주소
    - https://github.com/comstudy21joon/addinedu

- heroku(무료)
    - firebase와 유사하다. 파이썬이나 노드js를 사용 가능하다
        - 사이트 주소 https://www.heroku.com/

- firebase는 노드js를 사용하려면 functions를 사용하는데 이거는 유료다. 때문에 nodejs를 하는건 AWS가 더 유용하다.

- 클라우드 서비스
    - 참고 자료
        - https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=acornedu&logNo=220965470213

    - iaas = AMW (아마존같은것) 
        - 서버를 직접 구축하고 사용 하는것

    - PaaS = 파이어베이스,히로쿠,윈도우 (지금 배우고 있는것)
        - 서버를 구축안하고 사용하는것(개발만 하면 되는것)

    - SaaS = 일반사용자가쓰는 클라우드서비스
        - 네이버클라우드 구글클라우드같은것

1. React와 Firebase 연동하기

    1). react 폴더로 선언하기 
        - cmd 창에 ' npx create-react-app . '
    
    2) git허브 하기
        - git init
        - git remote add origin 주소
        - git add .
        - git push -u origin master 

    3) firebase 설치
        - 8 버전 설치하기(react할 폴더에)
            - npm install -s firebase@8 (-s는 이곳에만 설치//@8은 8버전으로 설치 한다는 뜻)
            - firebase를 dependencies에 추가하기 위해서 설치해야한다
        
        - 여태까지 우리는 firebase를 CDN으로 사용했는데 REACT에서는 NPM방식으로 해야하기때문에
            모듈을 만들어 설정을 해야한다.

        - 나중에 만들어서 build를 한다. 그것으로 나온 결과를 통해서 firebase로 deploy를 한다 때문에 따로만드는게    편하다(같이 만들어도 되지만 복잡하게 꼬일 수 도 있기 때문에)
            - npm run build를 사용하면 모든결과물이 하나로 묶인다. 이걸 build 폴더로 옮겨서 firebase로 옮겨서 deploy를 한다 (보통 REACT 완성하고 사용한다)

        
        1) 다운이 끝나면 src 폴더에 fbase.js를 만든다

        2) fbase.js에 기록한다
            ```
            import firebase from "firebase/app"

            // 이 내용은 firebase에서 내가 만든 프로젝트 설정 아래에 npm에 있다
            const firebaseConfig = {
                apiKey: ,
                authDomain: ,
                projectId: ,
                storageBucket: ,
                messagingSenderId: ,
                appId: ,
                measurementId: 
            };        

            export default firebase.initializeApp(firebaseConfig)
                //default는 한개만 넘긴다. const는 여러개를 넘긴다
            ```
            
            - fbase.js로 들어가서 id값들을 (process.env.설정값)으로 바꾼다

        3) index.js 들어간다

            ```
            import firebase from './fbase'
            ```

            입력한다.

        4) 인증키는 .env파일에 숨겨두고 깃허브에 올라가지 공개안되게 해야한다.
            - apiKey와 appId는 필수적으로 숨겨주는게 좋다

            - 프로젝트 진행할 폴더에 .env 파일을 만든다
                ```
                REACT_APP_API_KEY= 
                REACT_APP_PROJECT_ID= 
                REACT_APP_MESSAGING_SENDER_ID= 
                REACT_APP_APP_ID=
                
                //REACT_APP(REACT_APP은 환경변수이다)_APP_ID(변수이름)
                id종류는 다가려주는게 좋다
                ```

            - .gitignore 파일에 #misc있는 곳에 .env 를 추가로 적어준다
                - .env를 add할때 제외한다는 것이다.


    4) src 폴더 아래에 components와 routes 폴더를 만든다
        - routes 폴더에는 Aut.js, EditProfile.js, Home.js, Profile.js를 만든다
            - Auth.js, EditProfile.js, Home.js, Profile.js에 코딩을 한다
                ```
                export default function Auth() {
                    return (
                        <div>Auth page</div>
                    );
                }  
                    // 나머지는 이름만 바꿔서하면된다. 화살표 함수 사용가능
                ```

        - components 폴더에는 기존에 App.js를 이동시키고 Router.js를 만든다
            - npm install -s react-router-dom 을한다

        
    5) Router.js 에 코딩을 한다
            ```
            import { BrowserRouter, Route, Routes} from "react-router-dom";
            import Home from "../routes/Home";
            import Profile from "../routes/Profile";
            import Auth from "../routes/Auth";


            export default function AppRouter () {
                return(
                    <BrowserRouter>
                        <Routes>
                            <Route exact path="/" element={<Home />}>                    
                            </Route>
                            <Route exact path="/profile" element={<Profile />}>                    
                            </Route>
                            <Route exact path="/auth" element={<Auth />}>                    
                            </Route>
                        </Routes>
                    </BrowserRouter>
                )
            }
            ```

    6) App.js도 수정을 한다
        ```
        import AppRouter from "./Router";


        function App() {
        return (
            <AppRouter />
        );
        }

        export default App;

        ```
