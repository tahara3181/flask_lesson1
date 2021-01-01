<iframe name="oauth2relay484350284" id="oauth2relay484350284" src="https://accounts.google.com/o/oauth2/postmessageRelay?parent=https%3A%2F%2Fcolab.research.google.com&amp;jsh=m%3B%2F_%2Fscs%2Fabc-static%2F_%2Fjs%2Fk%3Dgapi.gapi.en.GhYSaDTWhs4.O%2Fd%3D1%2Fct%3Dzgms%2Frs%3DAHpOoo_CcmyUNBPTBtz4hsH0C6OHKqodVQ%2Fm%3D__features__#rpctoken=835915230&amp;forcesecure=1" tabindex="-1" aria-hidden="true" style="color: rgb(33, 33, 33); font-family: Roboto, Noto, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial; width: 1px; height: 1px; position: absolute; top: -100px;"></iframe>

#  Cifa10で学習したモデルで画像判定

## 仮想環境作成(venv)

仮想環境作成のコマンドを実行すると、Homeフォルダ内（Windows は ユーザー名フォルダ内 ）に指定した名前のフォルダが出来上がり、その中に `venv` で必要なフォルダやファイルがインストールされます。**今回の仮想環境はflask_lessonとします。**

仮想環境を作成するコマンド

```
python -m venv flask_lesson
```

コマンドで出来上がったフォルダ（ここではflask_lesson）にカレントディレクトリを移動します。

### 仮想環境を実行

#### Macの場合

Macで`venv` の仮想環境を実行するコマンド

```
source bin/activate
```

#### Windowsの場合

Windowsの場合仮想環境を実行するコマンドはMacと違います。パスの区切りはスラッシュではなくバックスラッシュ `\` または `¥` マークを使います。

```
Scripts\activate
```

Windowsで`venv` の仮想環境を実行するコマンド`Scripts\activate`Windowsで仮想環境を実行時に出るエラー**「PSSecurityException」**が発生する場合があります。

この場合次のコマンドを実行します。

```
Set-ExecutionPolicy RemoteSigned -Scope Process 
```

実行ポリシーを聞かれますので、「Y」を入力再び仮想環境に入ります。

その後次のコマンドを実行します。

```
Scripts\activate
```

仮想環境を実行するとターミナルのカレントディレクトリの表示に仮想環境の表示が加わります。

#### 仮想環境の終了

仮想環境の実行を止める仮想環境の実行を止めるコマンド`deactivate`

### Flaskのインストール

Flaskと必要なライブラリのインストールを行います。

今回のアプリで使用するTensorFlowのバージョンは、ローカル環境では2.4.0をインストールし、Herokuには2.0.0をインストールします。

そのため、モデルの学習はTensorFlow2.0.0環境で作成しています。

#### Flaskインストール

```
pip install Flask
```

#### Flaskバージョン確認

Flaskのバージョン確認は次のようにPythonから確認します。

```
$ python
>> import flask
>> flask.__version__
```

'1.1.2'のようにバージョンが確認できます。

今回のFlaskバージョンは 1.1.2 での例です。

#### tensorflowインストール

```
pip install tensorflow==2.4.0
```

#### werkzeug

```
pip install Jinja2 redis Werkzeug
```

#### gunicorn のインストール

```
pip install gunicorn
```

#### jinja2のインストール

```
pip install jinja2
```

#### h5pyのインストール

```
pip install h5py
```

#### numpyのインストール

```
pip install numpy
```

#### pillowインストール

```
pip install pillow
```



## 画像判定アプリ作成

### 画像判定アプリ用フォルダとファイルの準備

1. flask_lesson フォルダ内に image_guess フォルダを新規作成
2. cd コマンドでカレントディレクトリを image_guess に移動
3. アプリ用コードを記述する guess.pyをimage_guess フォルダ内に作成
4. image_guess フォルダ内に static フォルダを作成
5. static フォルダ内に　images フォルダを作成
6. image_guess フォルダ内に templatesフォルダを作成
7. templatesフォルダ内にindex.html, layout.html, result.htmlファイルを作成

- Topページはテンプレートの index.html
- 判定ページは result.html
- 画像ファイルのアップロードは index.html で行います。



## ローカル環境でのサーバー起動

guess.pyのファイルがある場所にcdで移動後以下コマンド

```
python guess.py
```

- Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)と表示されますのでこのURLでブラウザを開きます。

サーバーを止めるには、表示に記述されている通り、CTRL+C です。

以上の表示でブラウザで確認します。



## デプロイ

### Procfile の準備

heroku 上で実行するコマンドを記載します。あるいは新規でファイルを作成して、Procfileというファイル名にします。 拡張子はありませんので注意してください。

記述内容の中の`guess:app`が重要です。

**必ずguessの部分はアプリのコードを記述したファイル名（拡張子は不要）にしてください。**

記述内容

```
web: gunicorn guess:app --log-file -
```

### requirements.txt の準備

**重要：今回は自分のパソコンの環境をそのまま書き出すとHerokuのインストールに失敗します。
必ず以下の記述にしてください。**

```
Flask==1.1.2
gunicorn==20.0.4
h5py==2.10.0
Jinja2==2.11.2
numpy==1.18.3
Pillow==7.1.2
tensorflow==2.0.0
Werkzeug==1.0.1
```

参考：自分の仮想環境のインストール情報を使う場合は以下の手順で作成します。

以下コマンド実行

```
pip freeze > requirements.txt
```

通常はこれで`requirements.txt`が出来上がります。

### runtime.txt

新規で runtime.txt を作成して以下の内容を記述

```
python-3.6.10
```

- tensorflow==2.0.0をインストールするためには、Pythonのバージョンが最新ではインストールできませんので注意してください。



### ファイル階層

この段階のファイル階層

```
.
├── Procfile
├── cifer.h5
├── guess.py
├── requirements.txt
├── runtime.txt
├── static
│   └── images
│       └── cat.jpg
└── templates
    ├── index.html
    ├── layout.html
    └── result.html
```

## Heroku CLIの導入

Heroku-CLIはターミナル上でHerokuアプリをより簡単に使えるツールです。

[ダウンロード](https://devcenter.heroku.com/articles/heroku-cli)こちらのページから自分のPC環境に合ったものでインストールします。

### ログイン

Herokuにログインは次のコマンドです。ログインはブラウザが立ち上がりますので、そこでログイン操作します。

```
heroku login
```

### Herokuのアプリ名を決めます。

**以降<アプリ名>記述部分は自分が付けたアプリ名が入ります。**

アプリの作成コマンド

```
heroku create <アプリ名>
```

エラーが出たら名前がすでに使われているので、 違う名前でアプリを作成してください。

次の内容が表示されるとOK

Creating ⬢ flask-vgg16... done

https://flask-vgg16.herokuapp.com/ | https://git.heroku.com/flask-vgg16.git

## GitでHeroku コミット

まずはgitにコミットします。

**staticフォルダ内のimagesフォルダにはローカルでテストした画像を入れたままaddします。空の状態だとフォルダがgitに反映されずにHerokuでフォルダが作成されません**

git初期化コマンド（既にgit管理している場合は必要ありません）

```
git init
```

Heroku用リモートの設定

```
heroku git:remote -a <アプリ名>
```

ステージにaddするコマンド

```
git add .
```

コミットするコマンド

```
git commit
```

herokuにpushするコマンド

```
git push heroku master
```

Herokuにプッシュが終わると、requirements.txtに従ってインストールが始まります。

一番のネックはTensorFlowのインストールです。2.1以上のバージョンだとコケます。

原因はSlug Sizeの問題です。

## インストールがうまくいかない場合

Herokuで作成したアプリには、Slug Size制限があります。

Slug SizeはブラウザからHerokuにログイン後、対象アプリ名を選択後グローバルナビゲーションの「Settings」から Info で判ります。

特にTensorFlowの最新版では500MB制限にかかりインストール出来ません。

TensorFlow2.0.0ではインストールに成功しています。

## 判定結果でエラーが出る場合

ColabのTensorFlowのバージョンやKerasの使い方次第でうまくHDF5ファイルが読み込めないことがあります。その場合ColabのTensorFlowのバージョンをHerokuにインストールするバージョンに合わせるなどの対策が必要になります。

ColabのTensorFlowをアンインストール

```
pip uninstall -y tensorflow
```

バージョン2.0.0のインストール

```
pip install tensorflow-gpu==2.0.0
```