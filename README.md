# 名城大学-亀谷研究室-ヒートマップ閲覧システムデモ

<img width="1680" alt="Screen Shot 2020-06-08 at 3 26 44" src="https://user-images.githubusercontent.com/44857986/83976883-e7794000-a937-11ea-8b8c-d080c7dd7d20.png">

## 起動のための手順1 : Node.jsをインストールする
### [Node.jsのHP](https://nodejs.org/)からダウンロードする。画面左の推奨版を推奨。

## 起動のための手順2 : npm installを実行する
```
npm install
```

## 起動のための手順3 : npm run serve で開発用web サーバを起動する
```
npm run serve
```

## 起動のための手順3: http://localhost:(表示されているポート番号) へWeb ブラウザでアクセス

# デモを動かすための操作手順

### ※システムがまだ未熟なので,この手順通りに動かさないと,エラーが起きる可能性があります
## 手順1 : トグルボタンのエポック数"100" ~ "500"から一つを選択
### (画像では100を選択)
<img width="803" alt="Screen Shot 2020-06-08 at 2 55 55" src="https://user-images.githubusercontent.com/44857986/83976259-e0e8c980-a933-11ea-86b4-187a1d42836e.png">

## 手順2 : フォルダ"dataset"から手順1で選択したエポック数に該当する画像データセットをアップロードする
### (ファイル名で"...ep〜... "の〜の部分がエポック数を表しています.手順1で"100"を選択したので"...ep100... "を選択しています)
<img width="1303" alt="Screen Shot 2020-06-08 at 3 08 51" src="https://user-images.githubusercontent.com/44857986/83976860-c6185400-a937-11ea-854c-3974b0740de8.png">

## 手順3:  手順1~2を別のエポック数でもう一度繰り返す

# ブラウズ方法
## 手順1:  左右のサイドバーからエポック数を選択する
## 手順2 : 画像が表示されるので可視化結果を閲覧したい画像を選択する
## 手順3 : ボタン"epoch"を選択すると学習経過を動画で閲覧できる
## 手順4 : 学習経過の画面でサイドバーより"method"を選択すると別の可視化手法との学習経過を比較することができる

## 手順5:  左右のサイドバーからエポック数を選択すると元の画面に戻る