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
```rb
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

## .rspecに設定を追加
**.rspec**
```ruby:.rspec
--require spec_helper
--format documentation
```

<br>
<br>

## テストコードを記述するファイルを用意
**ターミナル**
```
% rails g rspec:model user
```
<br>


```ruby:spec/models/user_spec.rb
require 'rails_helper'

RSpec.describe User, type: :model do
  pending "add some examples to (or delete) #{__FILE__}"
end
```
RSpecでモデル、ビュー、コントローラーのテストを行うためには、rails_helper.rbというファイルを読み込む必要がある。
<br>
<br>

### rails_helper
Rspecを用いてRailsの機能をテストするときに、共通の設定を書いておくファイル。各テスト用ファイルでspec/rails_helper.rbを読み込むことで、共通の設定やメソッドを適用できる。
<br>
rails gコマンドでテストファイルを生成すると、rails_helperを読み込む記述が、自動的に追加。

<br>

# モデル単体テスト

<br>

## テストコードの雛形

**describe**

describeとは、テストコードのグループ分けを行うメソッド。
「どの機能に対してのテストを行うか」をdescribeでグループ分けし、その中に各テストコードを記述。
<br>

```ruby:spec/models/user_spec.rb
require 'rails_helper'

RSpec.describe User, type: :model do
  describe 'ユーザー新規登録' do
    # ユーザー新規登録についてのテストコードを記述  
  end
end
```






<br>
<br>
<br>
<br>
<br>

## 結合テストコード
ユーザーがブラウザで操作する一連の流れを再現して、問題がないか確かめるやり方  



