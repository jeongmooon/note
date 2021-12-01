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
    7) 구글 아이디와 깃허브 추가하기
        - firebase 홈페이지에 들어간다

        - AuthenTication에 들어가서 Sing-in method에 들어가 새 제공탭에서 구글, 이메일 , 깃허브를 추가한다

        - 깃허브 추가방법은 깃허브를 누르고 url을 복사한 후에 깃허브 홈페이지들어가서 setting 탭으로 들어가 oAuth에 들어가서 url을 집어넣고 계정을 복사해서 넣고 비밀번호를 넣는다


2. firebase 요소들 가져오는 방법
    - fbase.js에 들어간다.
    - 사용할 요소들을 import 하고 export 한다
        ```        
        import "firebase/auth"; (계정)
        import "firebase/firesotre"; (데이터베이스 저장)
        import "firebase/storage"; (파일 저장)

        export const authService = firebase.auth();
        export const storageService = firebase.storage();
        export const dbService = firebase.firestore();
        export const firebaseInstance = firebase;

        ```

    - storage와 firesotre(db)의 차이점은?
        - storage는 파일 또는 물리적 저장소의 객체 스토리가 될 수 있다
        - db는 조직화된 데이터가 저장된 장소이다

        - db는 storage의 일종이다

        - sotrage는 텍스트, 이미지, 영상등 다양한 종류가 저장이가능하다
        - db는 ID, record, 거래정보 같은 구조적 또는 반구조적 데이터가 저장이된다.

        - 즉, 스토리지는 파일형태가 되면 무엇이든 담을 수 있고, DB는 담기위해서 가공해야한다. 예를 들면 글의 제목과 내용은 DB에 저장이 되지만, 업로드 파일들은 스토리지에 저장이 된다.

    - firebase 요소가 필요한곳에 import해주면 끝난다
        ```
        만약에 Auth.js에서 authService와 firebaseInstance를 사용 하고 싶다면
        Auth.js에 들어간후에

        import { authService, firebaseInstance //사용할 요소들 } from "fbase";

        를 입력하면 된다
        ```


3. src 기본 베이스 경로로 지정하기
    - 제일 상단 폴더에 전역으로 작용하도록 파일을 만든다. 
    - jsconfig.json으로 만들었다
        ```
        {
            "compilerOptions": {
                "baseUrl": "src"
            },
            "include": ["src"]
        }
        ```
        를 입력하면 된다.


4. 네비 바 만들기
    - componets 폴더에 Nav.js를 만든다

    - 네비바는 웹 사이트에서 가장 많이 클릭하는 것으로 모든페이지에 들어가는게 좋다

    - 메뉴를 의미하는 것으로 보기도 좋고 편리하게 만들수 있다

    - React에서 react-router를 이용해 페이지 전환을 하기위해서는 react-router-dom의 Link 요소를 이용해야한다
        ```
        import { Link } from "react-router-dom";
        ```
        이걸 상단부에 입력을 한다.

    - 나머지는 다른것과 똑같이 함수를 만들고 export 시킨다.


5. App.js 설정하기
    - 이곳을 통해서 설정한 요소들을 모아서 index.js로 이동하여 웹을 구성한다

    - 메인페이지 같은 것이다

6. Router.js 
    - Route는 페이지를 연결시켜주는 것이다.

    - Router.js는 그런 파일은 아니지만 App.js에 입력이 많다면 복잡해서 중간다리로 생각하고 만들었다.

    - 중간다리로 다른요소들을 가지고와서 조합한다.(firebase,메인페이지,Navi,회원가입등등)

    - react-router-dom에 내장된 컴포넌트들(BrowserRouter, Routes, Route, Link)
        - BrowserRouter : html5의 history API를 통해 UI를 업데이트한다
        - Routes : Route로 생성된 자식요소 중에 매칭되는 첫번째 Route를 렌더링해준다 이것을 이용해 특정 컴포넌트만 렌더링 해서 화면에 띄울 수 있다.
        - Route : 요소별로 원하는 url을 지정한다.
        - Link: 지정한 URL로 이동하게 해준다 새로운페이지를 불러오는 것이므로 기존 요소들의 상태값은 소멸된다.
    
    - 로그인하고 로그아웃 만들기
        ```
        // 내장컴포넌트가져오기
        import { BrowserRouter, Routes, Route } from "react-router-dom";

        // 가지고온 요소들
        import Home from "routes/Home";
        import Profile from "routes/Profile";
        import Auth from "routes/Auth";
        import EditProfile from "routes/EditProfile";

        //네비바
        import Nav from "components/Nav";

        export default function AppRouter({ isLoggedIn, setIsLoggedIn //가지고갈 요소 }) {
        return (
            <BrowserRouter>
            <Nav />
            <Routes>
                {isLoggedIn ? ( // 로그인이 된다면
                <>
                    <Route exact path="/" element={<Home />} />
                    <Route path="/profile" element={<Profile />} />
                    <Route path="/edit" element={<EditProfile />} />
                </>
                ) : (   // 로그인 상태가 아니라면 회원가입을 띄운다
                <Route exact path="*" element={<Auth />} />
                )}
            </Routes>
            </BrowserRouter>
        );
        }       

        ```

7. 회원가입과 로그인 만들기
    - Auth.js가 로그인상태가 아니라면 띄우는 것으로 여기에 구성한다

    - firebase에서 회원가입서비스와 firebase 객체를 사용해야한다

    - Hook 구조분해이다
        - 참고사이트 https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment

        ```
        //Hook에서 요소를 빼내서 선언하는 것
        var ~~~ = [요소, 요소, 요소];        
        ```

    - useState를 사용해서 클래스 기반 컴포넌트함수를 제작한다. 첫번째 원소는 상태값을 저장할 변수고, 두번째 원소는 상태값을 갱신할 때 사용 할 함수다.
        - 상태 값 갱신 함수를 사용하지 않고 직접 젼수를 다른 상태값으로 하면 반영이 안된다
        - 클래스 컴포넌트에서 state갱신할 때는 반드시 setState를 사용해야 한다.
        - 값이 바뀌면 자동으로 렌더링 된다.

    - firebase에 저장된 SNS아이디로 로그인하기
        - SNS 함수는 firebase auth에 저장된 함수를 부르면된다.
        ```

        const onSnsLogin = async function (event) {
            
            // fbase.js에  firebase 객체를 사용하도록 설정 필요하다.
            var provider;
            if (event.target.name === "google") {
            provider = new firebaseInstance.auth.GoogleAuthProvider();
            } else if (event.target.name === "github") {
            provider = new firebaseInstance.auth.GithubAuthProvider();
            }
            const data = await authService.signInWithPopup(provider);
            console.log(data);
        };

        ```

    -
8. 로그아웃 처리
    - 로그인 후에는 Home.js 가 나오는데 로그아웃 버튼을 만든다
    - 그 후에 파이어베이스에서 있는 로그아웃 처리를 사용한다

    ```
    <button
        onClick={function (event) {
          authService.signOut(); // 파이어베이스 로그아웃 처리
        }}>
        Logout
    </button>
    ```


11.30

li에 key값

1. 복습한 것
    - 보내 줄 요소들 한테 태그에 추가한다
    ```
    하위 요소에 있는 Message에 msg, userObj를 주고싶다 하면 태그에 이렇게 추가하면 된다
    ex) <Message msg={msg} userObj={userObj} />
    ```

    - useState를 많이 사용한다. 다시 확인해보기

    - true flase로 로그인하고 안했다면 나오는 버튼 만들기
        - 구조
        ```
        {상태값 ? (
            <나타낼 html>            
        ) : (
            <아닐때 나올 html>
        )}
        ```

        - 적용 예시
        ```
        <li>
            {isLoggedIn ? ( // 로그인이 되어있다면
            <button
                onClick={function (event) {
                authService.signOut(); // 파이어베이스 로그아웃 처리
                }}
            >
                Logout
            </button>
            ) : ( // 로그아웃이라면
            <>로그인 되어있지 않음</>
            )}
        </li>
        ```


2. 입력된 글을 삭제하기
    - 함수 만들기
        - async로 클릭시 페이지가 안바뀌게 하기
        - objId로 대상지정 후 삭제
        
    예시)
    ```
    1번 방법
    async function onDelClick(objId) {        
        const data = await dbService.doc(`messages/${objId}`).delete();
    }

    2번 방법
    async function onDelClick(objId) {
        const data = await dbService.collection("messages").doc(objId).delete();
    }

    버튼에 클릭시 함수가 실행해주도록 세팅
    <button
        onClick={function (event) {
            onDelClick(msg.id);
        }}
        >
        삭제
    </button>
    ```
    
3. 로그인된 유저가 맞다면 자기가 쓴글 수정하거나 삭제하기
    - 복습했던 조건문 사용

    - 이번엔 로그인된 유저(userObj.uid)가 글을쓴 유저(creatorId)가 동일인인지 확인

    ```
    <li>
      <span style={{ display: "inline-block", width: "400px" }}>
        {editing ? (
          <input
            type="text"
            value={newMsg}
            onChange={function (event) {
              setNewMsg(event.target.value);
            }}
          />
        ) : (
          <>
            {msg.text}({msg.creatorEmail})
          </>
        )}
      </span>
      {userObj.uid === msg.creatorId ? (
        <>
          <button
            onClick={function (event) {
                setEditing(!editing);
                if (editing){
                    dbService.doc(`message/${msg.id}`).update({text:newMsg})
                }
            }}
          >
            {editing ? "수정완료" : "수정"}
          </button>
          <button
            onClick={function (event) {
              onDelClick(msg.id);
            }}
          >
            삭제
          </button>
        </>
      ) : (
        <></>
      )}
    </li>
    ```

4. 입력된 이미지 실시간 변경하기
    - 실시간 갱신되도록 이벤트 핸들러 사용
        - .get() 대신 .onSnapshot()으로 대체한다
    
    예시
    ```
    useEffect(() => {

        dbService.collection("messages").onSnapshot((snapshot) => {
        const newMsgs = snapshot.docs.map((doc) => {
            return {
            id: doc.id, 
            ...doc.data(), // ...은 구조분해로 기존데이터를 복사해서 사용한다
            };             // 뒤에 놓으면 뒤로 복사해 붙인다
        });
        setMsgs(newMsgs);
        });
    }, []);
    ```

5. 이미지 미리보기, 삭제, 업로드 만들기
    - 파일입력과 삭제를 html만들기
    ```
    //이미지 파일 넣는 버튼생성(image/* 은 image를 모두포함)
    <input type="file" id="image" accept="image/*" onChange={onLoadImg} /> 

    <div id="prev_image">선택 사진 미리보기 :<img width="150" src={imgFiles}    alt="preview" /></div>
      <button onClick={onClearImg}>이미지 삭제</button>

    ```

    - 이미지 임의 URL 생성하기
        - useState를 사용한다
        ```
        const [imgFiles, setImgFiles] = useState("");
        ```

        - input된 이미지 URL을 가져온다
            - onChange로 함수넣는다

        ```
        //URL을 생성(URL.createObjectURL)하고 타겟된 파일을 지정(.files[0])
        onChange((event) => {
            setImgFiles(URL.createObjectURL(event.target.files[0]));
        })
        
        ```

        - 이 URL로 <img> 태그에 src로 경로지정한다

    - 미리보기 이미지 삭제
        - 저장된 URL을 null로 만든다
    
        ```
        function onClearImg() {
            setImgFiles(null);    
        }
        ```

    - 업로드 onSumbit함수 수정
        - 파일이름 지정을 위해 시간과 파일 끝부분 데이터 추출
        ```

        var 시간값 = new Date().getTime()
        var 끝부분 = 이미지.name.substr(이미지.name.lastIndexOf("."))
        var 저장이름 = 시간값 + 끝부분
        
        스토리지에 저장하기
        var 경로값 = storageServic.ref().child(저장이름)
        var 결과 = await 경로값.put(이미지파일)
        db쓸값 = 결과.ref.getDownloadURL()

        그리고 db에 저장되는 곳에
        imageUrl : 쓸값

        ```



12.01

- 강사님 node.js & mongodb 수업 Git 주소
    - https://github.com/comstudy21joon/addinedu_nodejs.git

1. node.js
    1) express
    2) view engine

    3)  노드 설정하기
        - npm init -y (노드 초기화)

        - public/ upload/ views 폴더 만들기

        - git 설정하기
            - git init
            - git remote add origin 경로
            - git add .
            - git commit -m"쓸말"
            - git push -u origin master

        - express 설치 
            - npm install express --save

        - git 이그노어 만들기
            - https://www.toptal.com/developers/gitignore
            - 들어가서 제외할 파일들 설정하고 생성 후에 다른이름 저장으로 만듬

        - nodemon 설치하면 매번 서버를 재실행 안해도됨
            - npm install nodemon --save-dev
            - pack.json 파일에서 script 부분 수정
    
    4) views 폴더에서 ejs나 pug로 사용하면 편함(서브 페이지)
        - view엔진은 서버사이드기술(백앤드)
        - react는 프론트앤드로 다르다
        - ejs 인스톨하기
            - npm install --save ejs (node_module안에 ejs를 저장한다)
    
    5) Post 방식 사용하기
        - // bodyParser 미들웨어 사용 - POST방식 전송에서 body의 데이터를 접근 가능해진다
            ```
            app.use(express.json());
            app.use(express.urlencoded({ extended:true }));
            ```

        - url 변수 받는방법
            - <a href "주소/:(id) ?(id))">
            - : 클론은 반드시 붙여야 한다
            - :(id)는 params로 받는 방법
            - ?(id)는 query로 받는 방법

2. 서버 구축하기
    - express 선언
        ```
        const express = require("express");
        const app = express();        
        ```

    - 라우터 선언
        ```
        const router = express.Router();
        ```

    - 서버생성
        ```
        app.use("/", router);
        const server = http.createServer(app);
        server.listen(3000, () => {
        console.log("서버 실행 중 : http://localhost:3000");
        });
        ```
    
    - views엔진 사용
        ```
        app.set("view engine", "ejs");
        app.set("views", __dirname + "/views");
        ```

    - get방식 기본페이지 틀
        - req와 res순서가 바뀌어선 안된다
        ```
        router.route("/").get((req, res) =>{
            res.end();
        })
        ```
    
    1)  파일을 넣기(read)
        - 선언된 파일 리스트가 있을때 html로 뿌려주는 방법
        - 예를 들어 car_list가 있다
        - 그것을 car_list.ejs에 넣고 싶다
        
        ```
        router.route("/주소").get((req, res) =>{
            var 데이터명 = {title:"list", list: list };

            req.app.render("list", 데이터명, (err, html)=>{
                res.end(html); //res 해야함
            })
        })
        ```
    
    2) 파일 만들기(create)
        - 보내준 파일을 업데이트
        - post 방식사용
        - post는 body에 정보를 받아옴

        ```
        router.route("/주소").post((req,res)=>{
            var 데이터명 ={
                필드명 : req.body.가져온필드명
                ...
            }

            데이터테이블.push(데이터명) // 데이터 만드는 작업
            res.redirect("/이동url경로") // 경로로 새로고침한다 "/경로"에서 /는 절대경로
        })
        ```

    3) 파일 상세보기
        - 상세보기는 read와 같은 get 방식
            ```
            router.route("/주소/:고유번호").get((req, res) =>{
                function 함수명(구유번호) {
                var no = Number(고유번호);
                var 데이터 = null;
                for (var i = 0; i < car_list.length; i++) {
                    if (car_list[i].no == no) {
                    carData = car_list[i];
                    break;
                    }
                }
                return carData;
                }
                var 데이터 = 함수명(req.params.고유번호); //params로 번호를 들고왔기 때문
                req.app.render("car_detail", {car:데이터명}, (err, html) =>{
                    res.end(html); //read와 똑같이 끝남
                })
            })
            ```

    4) 상세보기에서 수정하기(update)
        - 수정은 read와 같은 get으로 저장된 데이터를 가지고온 후에 create처럼 post로 보내서 수정을 한다
            ```
            router.route("/car_modify/:no").get((req, res) => {            

            var carData = findCarData(req.params.no);
                req.app.render("car_modify", { car: carData }, (err, html) => {
                    res.end(html);
                });
            });

            router.route("/car_modify").post((req, res) => {

            // 수정된 정보가 반영
            var carData = {
                no: req.body.no,
                name: req.body.name,
                price: req.body.price,
                company: req.body.company,
                year: req.body.year,
            };
            var idx = findIndex(req.body.no);
            car_list[idx] = carData;

            // 목록 페이지로 갱신
            res.redirect("/car_list");            
            });
            ```

        - 이렇게 두개의 router를 만든다

    5) 삭제하기(deleta)
        - post 방식으로 보내서 지운다

        ```
        router.route("/car_delete/:no").post((req,res) =>{

            var no = findIndex(req.params.no);
            car_list[idx] = carData;
            
            car_list.delete(req.body.no);
            res.redirect("/car_list"); // 페이지 갱신하기
        });
        ```