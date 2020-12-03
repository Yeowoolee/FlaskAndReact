FLASK AND REACT
==================

플라스크와 리액트를 함께 사용해보자

**개발환경 구성**
--------------------

node.js와 python, flask를 설치

Flask, React 파일을 분리한다.
flask-backend와 react-frontend로 분리했다.

홈 디렉토리에서 아래 명령어를 실행
>>> create-react-app react-frontend

react-frontend 디렉토리에서 create-react-app을 한 것과 같다.

react-frontend 디렉토리에서 아래 명령어를 실행
>>> npm run eject 

 eject는 해당 프로젝트에 걸려서 숨겨져 있는 모든 설정을 밖으로 추출해주는 명령어

react-frontend/config/jest/paths.js 파일을 아래와 같이 수정

.. code::

      appBuild: resolveApp('../flask-backend/static/react')

react-frontend/config/jest/webpack.config.js 파일을 아래와 같이 수정

.. code::

    new MiniCssExtractPlugin({
          // Options similar to the same options in webpackOptions.output
          // both options are optional
          filename: 'css/[name].[contenthash:8].css',
          chunkFilename: 'css/[name].[contenthash:8].chunk.css',
        })

.. code::

    new HtmlWebpackPlugin(
        Object.assign(
          {},
          {
            inject: true,
            template: paths.appHtml,
            filename: "../../templates/index.html",
          }

파일 경로 전체 수정 
static/ 이라고 된 경로 부분을 전부 공백으로 변경

package.json파일을 아래와 같이 수정, homepage를 추가

.. code::

    {
  "name": "react-frontend",
  "version": "0.1.0",
  "private": true,
  "homepage": "/static/react",
  "dependencies": {
    "@babel/core": "7.12.3",
    .
    .
    .


**Flask 변수를 전달하기**
--------------------------

**main.py**

.. code::

    def main():
        return render_template("index.html", token="Hello Flask and React")


**public 디렉토리의 index.html**

head 안에 아래 코드 추가

>>> <script>window.token = "{{token}}"</script>

**App.js**

아래와 같이 token 추가

.. code::

    function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <p>My Token = {window.token}</p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

**빌드**
---------

아래 명령어로 빌드해서 서버에서 표시할 html파일을 만든다.

>>> npm run build

flask서버를 실행시키고 token이 잘 보내졌는지 확인한다.

