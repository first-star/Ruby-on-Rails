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
```rb:
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
```ruby:
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

**spec/models/user_spec.rb**
```ruby:
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

**spec/models/user_spec.rb**
```ruby:
require 'rails_helper'

RSpec.describe User, type: :model do
  describe 'ユーザー新規登録' do
    # ユーザー新規登録についてのテストコードを記述  
  end
end
```
<br>

**it**
itメソッドは、describeメソッド同様に、グループ分けを行うメソッド。
itの場合はより詳細に、「describeメソッドに記述した機能において、どのような状況のテストを行うか」を明記。

itメソッドで分けたグループを、exampleという。

<br>

**example**
exampleとは、itで分けたグループのこと。
また、itに記述した内容のことを指す。
<br>

(例)
* nicknameが空では登録できない
* emailが空では登録できない
<br>

**spec/models/user_spec.rb**
```ruby:
require 'rails_helper'

RSpec.describe User, type: :model do
  describe 'ユーザー新規登録' do
    it 'nicknameが空では登録できない' do
      # nicknameが空では登録できないテストコードを記述します
    end
    it 'emailが空では登録できない' do
      # emailが空では登録できないテストコードを記述します
    end
  end
end
```
<br>
<br>

## テストコードを実行
**ターミナル**
```
% bundle exec rspec spec/models/user_spec.rb
```
<br>
結果が緑色で表示されれば実行成功

<br>
<br>

## nicknameが空の場合の記述
異常系のモデル単体テストの実装は、以下の流れ。

1. 保存するデータ（インスタンス）を作成する
2. 作成したデータ（インスタンス）が、保存されるかどうかを確認する
3. 保存されない場合、生成されるエラーメッセージが想定されるものかどうかを確認する
<br>

**検証のためのインスタンスを作成**
<br>
nicknameのバリデーションに指定されている、presence: trueの挙動を検証
<br>

**spec/models/user_spec.rb**
```ruby:
require 'rails_helper'
RSpec.describe User, type: :model do
  describe 'ユーザー新規登録' do
    it 'nicknameが空だと登録できない' do
      user = User.new(nickname: '', email: 'test@example', password: '000000',password_confirmation: '000000')
    end

    it 'emailが空では登録できない' do
    end
  end
end
```
nicknameの値が空のインスタンスを生成。

この後、nicknameに設定されているpresence: tureが正常に機能するか検証するため、バリデーションを実行。バリデーションはDBに保存する前しか実行されない。
valid?メソッドを用いて、任意のタイミングでバリデーションを実行する。
<br>

* valid?
<br>
valid?は、バリデーションを実行させて、エラーがあるかどうかを判断するメソッド。
エラーがない場合はtrueを、ある場合はfalseを返す。
エラーがあると判断された場合は、エラーの内容を示すエラーメッセージを生成。

(例)Userモデルにおいてnicknameにはpresence: trueのバリデーションが設けられている。

```ruby:app/models/user.rb
class User < ApplicationRecord
# 省略
validates :nickname, presence: true
# 省略
end

```

<br>

**ターミナル**
```
% rails c
[1] pry(main)> user = User.new(nickname: '')
[2] pry(main)> user.valid?
=> false
```
<br>
<br>

**生成したインスタンスに対してバリデーションを実行**
<br>

**spec/models/user_spec.rb**
```ruby:
require 'rails_helper'

RSpec.describe User, type: :model do
  describe 'ユーザー新規登録' do
    it 'nicknameが空だと登録できない' do
      user = User.new(nickname: '', email: 'test@example', password: '000000', password_confirmation: '000000')
     user.valid? #this
    end
    it 'emailが空では登録できない' do
    end
  end
end

```
nicknameが空のインスタンスuserに、valid?メソッドを使用

Userモデルのバリデーションにはpresence: trueが指定されているため、nicknameが空ではDBに保存できないと判断され、valid?メソッドはfalseを返すはずです。

「想定した挙動」と「アプリの実際の挙動」をテストコードに落とし込んだものをexpectationという
<br>

## expectation

検証で得られた挙動が想定通りなのかを確認する構文

テストの内容に応じて引数やmatcherを変えて記述
expect().to matcher()
<br>

## matcher
matcherは、「expectの引数」と「想定した挙動」が一致しているかどうかを判断。
expectの引数には検証で得られた実際の挙動を指定し、マッチャには、想定している挙動を記述。

代表的なのは`include`と`eq`マッチャ
<br>


### include
includeは、「expectの引数」に「includeの引数」が含まれていることを確認するマッチャ。
<br>

**(例)includeマッチャのテストコード**
```ruby
describe 'フルーツ盛り合わせ' do
  it 'フルーツ盛り合わせにメロンが含まれている' do
     expect(['りんご', 'バナナ', 'ぶどう', 'メロン']).to include('メロン')
  end
end
```

配列に、メロンが含まれることを想定
配列のなかにメロンは含まれているため、テストは成功



























<br>
<br>
<br>
<br>
<br>

## 結合テストコード
ユーザーがブラウザで操作する一連の流れを再現して、問題がないか確かめるやり方  



