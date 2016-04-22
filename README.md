# AngulaJSハンズオン

## ディレクトリを作成します

```
ng_todo
    ∟ index.html
```

## index.html の準備

- HTML の記述
- [Angular公式HP](https://angularjs.org/)からAngular の読み込み(CDN)

  `http://ajax.googleapis.com/ajax/libs/angularjs/1.5.3/angular.min.js`

```
<!DOCTYPE html>
<html lang="ja"ng-app >
<head>
    <meta charset="UTF-8">
    <title>Todoアプリ</title>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.3/angular.min.js"></script>
</head>
<body>
   
</body>
</html>
```

## AngularJSを動かしてみよう

- {{ 1+1 }} を入力してみよう
 → 2が表示されれば正常にAngularJSが動いている。
 
## Controllerを作成しよう。
-  angular.moduleを作成します。  

```
<script>

    angular.module("myApp", [])

</script>
```

- 作成したmoduleをng-appに設定します。  

```
<html lang="ja" ng-app="myApp">
```

- コントローラーを作成します。

```
<script>

    angular.module("myApp", [])
            .controller("appController", function() {
                var vm = this;   
            });
</script>
```

- controller as ** を指定する(Angular1.2から推奨されている方法)

```
<body ng-controller="appController as vm">
```

- 以上でcontrollerは作成されたので、処理を追加

```
<script>

    angular.module("myApp", [])
            .controller("appController", function() {
                var vm = this;

                vm.clickButton = function() {
                    alert('正常に動作');
                }
            });

</script>
```

- 追加した処理をボタンを押すことで発火させるようにします。

```
<div>
    <button ng-click="vm.clickButton()">起動ボタン</button>
</div>
```

- 正常にアラートが起動すれば成功！！

![スクリーンショット 2016-04-15 11.47.12.png](https://qiita-image-store.s3.amazonaws.com/0/108073/c6ca4115-b4c0-0fd2-675e-f5d4edabccfd.png "スクリーンショット 2016-04-15 11.47.12.png")

## ng-repeatを使用して配列を繰り返し表示しよう

- 表示する配列を作成します。

```
<script>

    angular.module('myApp', [])
            .controller('appController', function() {
                var vm = this;


                vm.fruits = [
                    "りんご",
                    "梨",
                    "ぶどう",
                    "柿",
                    "いちご",
                    "メロン",
                    "パイナップル"
                ];
            });

</script>
```

- ng-repeatを作成。

```
<div>
    <ul ng-repeat="fruit in vm.fruits">
        <li>{{fruit}}</li>
    </ul>
</div>
```

- この画面が表示されれば成功  

![スクリーンショット 2016-04-15 12.02.05.png](https://qiita-image-store.s3.amazonaws.com/0/108073/7fd6f464-f82b-ded0-07e5-ef2afd7f5891.png "スクリーンショット 2016-04-15 12.02.05.png")

## Todo項目を作成しよう

- Todo項目を格納する空の配列を用意します

```
<script>

    angular.module('myApp', [])
            .controller('appController', function() {

                var vm = this;

                vm.todos = [];
                
                vm.todoTitle = '';
            });

</script>
```

- 入力フォームと作成用ボタンします。

```
<div>
    <form>
            <input type="text" name="todo" placeholder="TODOを入力">
            <button>TODOを作成</button>
    </form>
</div>
```

- ボタンクリック時に発火させたい処理を実装します。

```
<script>

    angular.module('myApp', [])
            .controller('appController', function() {

                var vm = this;

                vm.todos = [];
                vm.todoTitle = ''

                /*
                 * TODO項目の作成処理
                 */
                vm.todoCreate = function() {
                	  var item = {
                        title: vm.todoTitle
                    };
                    vm.todos.push(item);
                    
                }
            });

</script>
```

- 入力フォームにng-modelを記述し、ボタンにng-clickを記述。
- ng-repeatを使用してTODO配列を表示させます。

```
<div>
    <form>
        <input type="text" ng-model="vm.todoTitle" placeholder="TODOを入力">
        <button ng-click="vm.todoCreate()">TODOを作成</button>
    	<ul ng-repeat="todo in vm.todos">
       		 <li>{{todo.title}}</li>
    	</ul>
    </form>
</div>
```

- 入力したデータが表示されれば成功

![スクリーンショット 2016-04-15 15.03.32.png](https://qiita-image-store.s3.amazonaws.com/0/108073/3f583c4e-18ed-5b6d-4a6d-421bf228ae8f.png "スクリーンショット 2016-04-15 15.03.32.png")

## 完了したTODO項目に打ち消し線をつけよう
- 配列に追加するデータにtrue/false項目を追加

```
<script>

    angular.module('myApp', [])
            .controller('appController', function() {

                var vm = this;

                vm.todos = [];
                vm.todoTitle = '';

                /*
                 * TODO項目の作成処理
                 */
                vm.todoCreate = function() {
                		vm.todoItem = {
                        title: vm.todoTitle,
                        done: false
                    };
                		vm.todos.push(vm.todoItem);
                    
                };
            });
</script>
```

- 完了チェックを行うチェックボックスを作成

```
<div>
    <form>
        <input type="text" ng-model="vm.todoTitle" placeholder="TODOを入力">
        <button ng-click="vm.todoCreate()">TODOを作成</button>
        <ul ng-repeat="todo in vm.todos">
            <li>
               <input type="checkbox" ng-model="todo.done">
              {{todo.title}}
            </li>
        </ul>
    </form>
</div>
```

- TODO項目にng-classを実装

```
<div>
    <form>
        <input type="text" ng-model="vm.todoTitle" placeholder="TODOを入力">
        <button ng-click="vm.todoCreate()">TODOを作成</button>
        <ul ng-repeat="todo in vm.todos">
            <li>
                <input type="checkbox" ng-model="todo.done">
                <span ng-class="{'done': todo.done }">{{todo.title}}</span>
            </li>
        </ul>
    </form>
</div>
```

- class="done"にスタイルを追加

```
<style>
    .done {
        text-decoration: line-through;
        color: red;
    }
</style>
```

- チェックをするとTODO項目に取り消し線が入力されれば成功  

![スクリーンショット 2016-04-15 16.16.59.png](https://qiita-image-store.s3.amazonaws.com/0/108073/143930ce-4c1c-371e-f783-7627f5acf099.png "スクリーンショット 2016-04-15 16.16.59.png")

## 完了したTODO項目を削除しよう

- 削除処理を作成
- angular.forEachを使用。[参考サイト](http://angularjsninja.com/blog/2013/12/06/angular-foreach/)

```
<script>

    angular.module('myApp', [])
            .controller('appController', function() {

                var vm = this;

                vm.todos = [];
                vm.todoTitle = '';

                /*
                 * TODO項目の作成処理
                 */
                vm.todoCreate = function() {
                    var item = {
                        title: vm.todoTitle,
                        done: false
                    };
                    vm.todos.push(item);

                    vm.todoTitle = '';
                };

                /*
                 * 削除処理
                 */
                vm.todoDelete = function() {
                    var newTodos = [];
                    angular.forEach(vm.todos, function(item) {
                        if (!item.done) {
                            newTodos.push(item);
                        }
                        vm.todos = newTodos;
                    });
                }
            });
            
</script>
```
- 削除ボタンを作成し、ng-clickを実装

```
<div>
    <form>
        <input type="text" ng-model="vm.todoTitle" placeholder="TODOを入力">
        <button ng-click="vm.todoCreate()">TODOを作成</button>
        <button ng-click="vm.todoDelete()">完了したTODOを削除</button>

        <ul ng-repeat="todo in vm.todos">
            <li>
                <input type="checkbox" ng-model="todo.done">
                <span ng-class="{'done': todo.done }">{{todo.title}}</span>
            </li>
        </ul>
    </form>
</div>
```

## Angular2への準備！1.5からの新機能Component化を実装！

- html用別ファイルを作成。

```
ng_Todo
    ∟ - index.html
      - todo.html
```

- todo.htmlの内容

```
<div>
    <form>
        <input type="text" ng-model="vm.todoTitle" placeholder="TODOを入力">
        <button ng-click="vm.todoCreate()">TODOを作成</button>
        <button ng-click="vm.todoDelete()">完了したTODOを削除</button>

        <ul ng-repeat="todo in vm.todos">
            <li>
                <input type="checkbox" ng-model="todo.done">
                <span ng-class="{'done': todo.done }">{{todo.title}}</span>
            </li>
        </ul>
    </form>
</div>
```

- index.htmlの内容をComponent化用に変更する

```
<script>
    angular.module('myApp', [])
            .component('todo', {
                controllerAs: 'vm',
                controller: function() {
                    var vm = this;

                    vm.todos = [];
                    vm.todoTitle = '';

                    /**
                     * TODO項目の作成処理
                     */
                    vm.todoCreate = function() {
                        var item = {
                            title: vm.todoTitle,
                            done: false
                        };
                        vm.todos.push(item);

                        vm.todoTitle = '';
                    };

                    /**
                     * 削除処理
                     */
                    vm.todoDelete = function() {
                        var newTodos = [];
                        angular.forEach(vm.todos, function(item){
                            if (!item.done) {
                                newTodos.push(item);
                            }
                        })
                        vm.todos = newTodos;
                    }
                },
                templateUrl: 'todo.html'
            });
</script>
```

- todoタグが使用できるようになったので、index.htmlに追加。
- Component化前と同じ画面が表示されれば、成功！

```
<body>

<todo></todo>

</body>
```
