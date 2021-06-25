# Rails テスト
<br>

### 正常系
「ユーザーが開発者の意図する操作を行った時の挙動」を確認するテストコードが、正常系に分類されます。

<br>

### 異常系
「ユーザーが開発者の意図しない操作を行った時の挙動」を確認するテストコードが、異常系に分類されます。たとえば、必須項目を入力せずに送信した際は、「必須項目を入力してください」とアラート画面などがでることがあります。この挙動を確認することは異常系に分類される。  

<br>

## 単体テストコード
モデル、コントローラー等機能ごとにテストするやり方  

<br>

# Gemを追加

**Gemfile**
```
group :development, :test do
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]
  gem 'rspec-rails', '~> 4.0.0'
end
```
<br>
<br>

**ターミナル**
```
% cd ~/projects/~~~
% bundle install
```
<br>
<br>

# RSpecの設定
<br>

## RSpecをインストール
**ターミナル**
```
% rails g rspec:install
```

<br>
<br>







<br>
<br>
<br>
<br>
<br>

## 結合テストコード
ユーザーがブラウザで操作する一連の流れを再現して、問題がないか確かめるやり方  



