## PHP 开发规范

本规范分为**编码格式篇**和**程序设计篇**两大部分。

# 编码格式篇

基于 PSR-1、PSR-2、PSR-12 。

### 样例：

```
<?php/** * this is a example class */declare(strict_types=1);namespace Vendor\Package;use Vendor\Package\{ClassA as A, ClassB, ClassC as C};use Vendor\Package\SomeNamespace\ClassD as D;use function Vendor\Package\{functionA, functionB, functionC};use const Vendor\Package\{ConstantA, ConstantB, ConstantC};class Foo extends Bar implements FooInterface{    public function sampleFunction(int $a, int $b = null): array    {        if ($a === $b) {            bar();        } elseif ($a > $b) {            $foo->bar($arg1);        } else {            BazClass::bar($arg2, $arg3);        }    }    final public static function bar()    {        // method body    }}
```

### 文件：

- PHP 代码**必须**使用 `` 标签，如果是纯 PHP 代码，则不带结束标签 `?>`；
- 编码：PHP 代码文件**必须**以不带 BOM 的 UTF-8 编码（关于 BOM 以及在 PHP 中的问题请自行百度）;
- `、`declare`、`namespace`、`use`块**必须**按照顺序编写，并且后面必须跟一个空行；
- `use`块：类、函数（`use function`）、常量（`use const`）的 `use`需按照此顺序书写，且每个小块之间必须有一空行；

### 行：

- 每行**不该**多于80个字符，大于80字符的行 应该 折成多行；
- 非空行后**一定不可**有多余的空格符；
- 每行**一定不可**存在多于一条语句

### 缩进：

- 代码**必须**使用4个空格符的缩进（请将 IDE 设置成 Tab 转 4 空格）；

### 关键字：

- PHP 关键字**必须**小写，且使用缩写形式（如使用 bool 而不是 boolean）;

### 命名：

- 类的命名**必须**符合首字母大写的驼峰规则；
- 方法和函数的命名**必须**符合首字母小写的驼峰规则；
- 常量命名**必须**全部大写，以下划线分割字母；
- 方法和属性**不可**用前导下划线表示其可访问性，而应当使用相应的访问修饰符；
- 类、方法、属性的名称应当能反映其意义，**禁止**使用诸如 `$a`、`$ddd` 这样毫无意义的命名；
- **应当**优先使用**业务概念**命名，**尽量避免**使用纯技术命名，如 sendCoupon 表示发券，属于业务用语，而 createUserCoupon 属于纯粹的技术用语；
- 在概念明晰的前提下，命名**应当**尽可能简洁，避免不必要的词语。如：相比 $orderList、ajaxGetOrderList，更好的命名是 $orders，getOrders；再如：UserCoupon::send() 优于 UserCoupon::sendCoupon()，前者恰好表达了其含义，而后者不必要地重复了词语 Coupon；
- **不应**使用通用的变量名，而应该使用具体的名称以增强可读性。如相对于使用 `$list`，`$users` 更符合上下文，更易于理解和维护；
- **不应**使用非通用的缩写，造成理解上的困难；
- **避免**使用纯技术要素的前后缀，如 ajaxGetOrders（作为一个接口，没必要也不应当限制消费者必须使用ajax）;
- **应当**使用名词复数表示集合，如应使用 $orders 表示订单列表而不是$orderList；

### 命名空间和类：

- 命名空间和类的命名必须符合 [PSR-4](https://www.php-fig.org/psr/psr-4/);
- 每个文件只定义一个类；
- 类命名：大写驼峰规则；
- 不要将类放到顶级命名空间中，至少需使用一层命名空间（一些特殊框架或历史项目可不遵守）；
- 创建类： `$cls = new MyClass();` 无论有无参数，都要加括号;
- `traits`: `use traits`：必须放在类左大括号下一行，每个 `trait` 单独一行，有自己的 `use`。use traits block 后面要有一个空行；

例：

```
class ClassName extends ParentClass implements \ArrayAccess, \Countable{    // constants, properties, methods}class ClassName extends ParentClass implements    \ArrayAccess,    \Countable,    \Serializable{    // constants, properties, methods}class ClassName{    use FirstTrait;    use SecondTrait;    use ThirdTrait;}class Talker{    use A, B, C {        B::smallTalk insteadof A;        A::bigTalk insteadof C;        C::mediumTalk as FooBar;    }}
```

### 类的常量、属性和方法：

- 常量：全部字母大写，用下划线分割，如 `ORDER_TYPE`；
- 属性：
  - 小写驼峰命名，如 `$order`；
  - **必须**使用访问修饰符，不可使用 `var` 修饰属性；
  - **不可使用**下划线开头来区分可访问性；
- 方法：
  - 小写驼峰，如 `submitOrder`;
  - **必须**使用访问修饰符；
  - **不可使用**下划线开头来区分可访问性；
  - 方法名称后**一定不可**有空格符；
  - 参数列表中，每个逗号后面**必须**要有一个空格，而逗号前面**一定不可**有空格;
  - 参数列表**可以**分列成多行，若这样，则包括第一个参数在内的每个参数都**必须**单独成行，并且结束括号以及方法开始花括号**必须**写在同一行，中间用一个空格分隔;

例：

```
    class ClassName    {        private $name ='lisi';        public function aVeryLongMethodName(                ClassTypeHint $arg1,                &$arg2,                array $arg3 = []            ) {                // 方法的内容            }    }
```

### 修饰符的使用：

- `abstract` 或 `final` 声明时，**必须**写在访问修饰符前;
- `static` **必须**写在其后;

例：

```
abstract class ClassName{    protected static $foo;    abstract protected function zim();    final public static function bar()    {        // method body    }}
```

### 方法和函数的调用：

- 方法及函数调用时，方法名或函数名与参数左括号之间**一定不可**有空格，参数右括号前也**一定不可**有空格。每个参数前**一定不可**有空格，但其后 必须 有一个空格。
- 参数**可以**分列成多行，此时包括第一个参数在内的每个参数都**必须**单独成行;

例：

```
bar();$foo->bar($arg1);Foo::bar($arg2, $arg3);$foo->bar(    $longArgument,    $longerArgument,    $muchLongerArgument);
```

### 控制结构：

- 控制结构关键词后**必须**有一个空格；
- 左括号 ( 后**一定不可**有空格；
- 右括号 ) 前也**一定不可**有空格；
- 右括号 ) 与开始花括号 { 间**必须**有一个空格；
- 结构体主体**必须**要有一次缩进；
- 结束花括号 }**必须**在结构体主体后单独成行；
- 每个结构体的主体都**必须**被包含在成对的花括号之中，哪怕只有一条语句；
- 使用关键词 `elseif` 代替 `else if`；
- if 断行：if 中条件过多，可每个条件一行，第一个条件需单独成行，boolean操作符要么全部放开头，要么全部结尾，不可混用;
- `switch`: `case` 语句 必须 相对 `switch` 进行一次缩进，而 `break` 语句以及 `case` 内的其它语句都 必须 相对 `case` 进行一次缩进;

例：

```
if ($expr1) {    // if body} elseif ($expr2) {    // elseif body} else {    // else body;}if (    $expr1    && $expr2) {    // if body} elseif (    $expr3    && $expr4) {    // elseif body}switch ($expr) {    case 0:        echo 'First case, with a break';        break;    case 1:        echo 'Second case, which falls through';        // no break    case 2:    case 3:    case 4:        echo 'Third case, return instead of break';        return;    default:        echo 'Default case';        break;}while ($expr) {    // structure body}for ($i = 0; $i < 10; $i++) {    // for body}foreach ($iterable as $key => $value) {    // foreach body}try {    // try body} catch (FirstExceptionType $e) {    // catch body} catch (OtherExceptionType $e) {    // catch body}
```

### 花括号的使用：

- 类和方法：起始和结束花括号**必须**单独一行，且起始花括号前后不能有空行；
- 流程控制语句：起始花括号不单独成行，结束花括号单独成行；
- 任何右打括号 `}` 后面不可跟注释或其它语句;

例：

```
class Foo extends Bar implements FooInterface{    public function sampleFunction($a, $b = null)    {        if ($a === $b) {            bar();        } elseif ($a > $b) {            $foo->bar($arg1);        } else {            BazClass::bar($arg2, $arg3);        }    }}
```

### 运算符：

- 所有的二元和三元运算符的前后**必须**各有一个空格;
- 一元运算符`!`后面不可有空格；

例：

```
if ($a === $b) {    $foo = $bar ?? $a ?? $b;} elseif ($a > $b) {    $variable = $foo ? 'foo' : 'bar';}
```

### 闭包：

- 闭包声明时，关键词 `function` 后以及关键词 `use` 的前后都**必须**要有一个空格；
- 开始花括号**必须**写在声明的同一行，结束花括号**必须**紧跟主体结束的下一行;
- 参数列表和变量列表的左括号后以及右括号前，**一定不可**有空格;
- 参数和变量列表中，逗号前**一定不可**有空格，而逗号后**必须**要有空格;
- 参数列表以及变量列表 可以 分成多行，这样，包括第一个在内的每个参数或变量都 必须 单独成行，而列表的右括号与闭包的开始花括号 必须 放在同一行;

例：

```
$closureWithArgs = function ($arg1, $arg2) {    // body};$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {    // body};$noArgs_longVars = function () use (    $longVar1,    $longerVar2,    $muchLongerVar3) {   // body};$longArgs_longVars = function (    $longArgument,    $longerArgument,    $muchLongerArgument) use (    $longVar1,    $longerVar2,    $muchLongerVar3) {   // body};$foo->bar(    $arg1,    function ($arg2) use ($var1) {        // body    },    $arg3);
```

### 代码注释：

- 类、方法、函数**必须**写注释；

- 类、方法**必须**使用块级注释，代码段视情况使用块级或行内注释；

- 注释**应当**包括功能说明、参数列表、返回类型、异常抛出情况；

- 注释文本和 // 之间有且只有一个空格；

- 比较复杂的代码段**应当**编写合适的注释；

- 不要写不必要的注释，比如下面的注释就是多余的：

  ```
  // 如果用户存在if ($user) {  // do something...}
  ```

# 程序设计篇

注：本规范没有考虑历史项目现状，历史项目可能在某些地方并不符合，可根据实际情况决定是否遵守。

### 异常：

- 异常的定义：凡是导致流程无法正常进行下去的，或者没有获取到预期结果的，都属于异常，例如除数的值是 0，获取用户信息接口没有查到用户；
- 代码中的异常**应当**抛出，而不应当以错误码的形式返回（除了最外层如控制器层，这层需要将异常转换成合适的格式输出给用户或日志。抛出异常而不是返回错误码遵循的原则是：业务逻辑和错误处理（非业务逻辑）分离，处理业务逻辑的代码只需将异常抛出(告诉上层)，上层可以处理该异常，也可以不处理（直接再给上层））；
- 异常应当包含明确的错误码和异常描述，其中错误码应当以常量的形式在项目中统一定义，而不应当以直接数字的形式写死（可读性、可维护性）；
- 控制器层**必须**捕获并以合适的方式处理异常，不能继续向上抛出。处理方式包括但不限于返回合适的错误码、记录日志、发告警通知等；

### 状态码/错误码：

- **不应当**在程序中直接写数字状态码，而应当在项目中统一的地方定义状态码常量（或类常量）；
- 状态码常量应当符合**命名**一节的规范描述；
- 不应当在非控制器层返回状态码，而应当以相应的异常代替（相应地，状态码体现在异常实例的 Code 上）；
- **不应当**使用通用状态码，每种错误应当定义自己的、唯一的状态码；
- 状态码应该在项目级别进行规划，不同的项目允许状态码重复，项目内部**不允许**不同的状态描述使用同一个状态码，反之，也**不允许**同一个状态描述使用不同的状态码；

### 日志：

- 原则上**应当**只在应用层（如应用层服务、控制器等）记录日志，尽量避免在领域层（业务逻辑层）记录日志，但该原则不做绝对要求；
- 日志内容包括但不限于：请求编号、请求详细内容、响应内容、错误发生的平台、错误描述、调用栈；
- 原则上所有的异常都**应当**有日志可追踪；
- **建议**对所有的外部请求以及本系统对外的 API 调用都做日志记录，用于出现异常情况时排查问题；
- 日志的实现**应当**遵循 [PSR-3](https://www.php-fig.org/psr/psr-3/) 日志接口规范；

### 缓存：

- **应当**为 js、html、css、image 等静态资源设置使用前端浏览器缓存（配置 nginx 或其他 Web 服务器）；
- **应当**对 js、html、css 资源开启压缩功能（配置 nginx 或其他 Web 服务器）；
- **应当**对经常访问但较少修改的数据使用内存缓存如 Redis、Memcache；
- 缓存的数据更新后**应当**及时更新/失效缓存；
- **应当**只缓存热数据，且设置合适的缓存期限。后端缓存建议过期时间不超过7天；
- **不应当**缓存大体量但并非全部热数据的数据；
- 后端缓存的实现**应当**遵循 [PSR-16](https://www.php-fig.org/psr/psr-16/) 缓存接口规范；

### 数据库：

- 数据表字段原则上**必须**添加注释，除非像 id、is_deleted 等大众皆知的字段；
- 表字段**不可**多义（一个字段表达多个业务含义，例如“用户登录表”用 user_id 是否为空表示用户是否登录，这里 user_id 表达了两层含义：用户标识和登录态。但需要区分的是，“多义”和“多值”是不同的，如用 status 字段通过多值与运算来存储多个状态，这里 status 的含义仍然是明确的）；
- 数据表的设计应该是“直白”的，**不应当**在字段上强加隐含的业务逻辑。例如上面的通过 user_id 是否为空来表示用户是否登录，就存在隐含业务逻辑，导致表结构的不稳定性（因为此时底层的存储结构依赖于上层的业务逻辑，而上层一般总是比底层不稳定）；
- 使用字符串存储 json 时必须仔细考虑其中字段是否可能会被检索，如果需要检索，则这种设计会带来麻烦；
- **必须**根据业务情况为表创建合适的索引，即使当前数据量不大（必须用动态眼光看待当前的情况，当前量不大不代表以后不大）；
- 原则上**禁止**在一次请求中对同一条数据先写后读，防止读写分离下数据不一致。如果必须这样做，建议在写入后 sleep 1-2 秒再读；
- **不应**使用 `*` 查询数据库字段，应当明确字段；
- 连表查询：四个表以上的关联需要慎重，且需要经过所在团队 2 个以上成员的审核；
- **禁止**直接操作非本系统/项目的数据库，必须调用相关接口，例如禁止在微信端直接操作券系统的数据库；
- 表字段：类似于 `last_update_time` 这样的字段**必须**设置 `on update current_timestamp`保证更新性；
- **禁止**在数据库事务中进行远程调用，这样会导致长事物，高并发下可能会导致数据库崩溃。解决方案：要么去掉事务，要么把远程调用拿到事务外面；

### 控制器：

- **禁止**在 Controller 中使用静态变量、静态方法。（完全没有必要，且在 easySwoole 等框架中容易出问题）；
- **禁止**在基类 `Controller` 中写 Action，即基类 `Controller` 不能对外提供 API（否则任何子类都拥有该 API，后面无法知道外界实际上到底访问了哪些控制器的该 API）；
- 基类控制器只能提供一些便捷属性和内部便捷方法，以及一些前后置处理逻辑，这些属性和方法都应当是 protected 级别的；
- **禁止**在控制器中写大量业务逻辑，应将其放入逻辑层，保持控制器层的简单；

### Session：

- Session **应当**仅仅存放“会话”信息，即会话上下文中必须使用的（公共）信息，其他信息应当用缓存存储。例如：商户平台登录者基本信息、所拥有的权限集、当前所在的层（集团、油站组、油站）等更登录会话密切相关的、公共的信息；
- **不应当**在领域层（业务逻辑）中直接使用 $_SESSION，而应当通过传参提供方法需要的东西。换句话说，只应当在应用层（如控制器）中使用 Session，防止 Session 污染；
- Session 的添加、修改应当在统一的地方进行，一般如登录成功后、退出登录、切换商户层级等，**禁止**在业务代码中随意设置 Session；

### API 接口：

- 对外的 API 接口**必须**有同步的、详细的文档，目前接口文档统一写在 showdoc 上面；
- API 接口的更新**必须**保证向前兼容性（除非能够确定调用方且能够相互协商修改）；
- 写型API（添加、更新、删除）**必须**保证多次调用的幂等性（如多次调用不会导致重复添加多条数据）,方便失败重试和手工补偿；
- API 返回的数据结构**必须**保证一致性，包括字段、结构一致性和数据类型一致性。如不可在某种情况下缺少某个字段，不同情况下某个字段类型不一致等；
- 所有的列表请求都**必须**支持分页，除非理论上不可能超过 50 条数据；

### 其它：

- **不应**在业务逻辑层写非本业务领域代码，而应当将其抽离成基础设施、本地服务或第三方接口（远程服务）。如虽然发送短信验证码属于用户注册流程的一环节，但发送短信验证码本身的逻辑不属于用户注册的业务领域，应将其抽离；
- **禁止**大段代码拷贝，应重构成方法或类；
- 一个方法或函数**不应**超过 120 行，一个类不应超过 800 行；
- 谨慎使用静态方法，因为从单元测试的角度一般认为静态方法不具有可测试性；
- 查询型方法**不应**产生副作用（修改系统状态、数据库记录、插入数据等）,只能返回相关数据（即保证查询方法的只读性）；
- 业务模型**不应**直接依赖于 GET、POST 等传入的参数，即不应将外界传入的参数直接丢给业务模型（甚至是直接插入数据库），业务模型**应当**显式定义自己需要的参数；
- 函数、方法参数的设计：
  - 方法的参数**应当**拥有自解释的能力，即每个参数拥有明确的含义；
  - 优先采用具有明确含义的多参数传递策略。如果参数数量过多，可采用传对象(DTO)的方式。尽量不要直接传递数组，因为数组元素不具有自解释性和约束性，不可维护，是下下策。
  - 例：用户登录校验传参：
    - 推荐：`$login->verify($username, $password);` 多参数传参，具有自解释性；
    - 如果参数过多（如超过 7 个），采用传对象方式：`$login->verify(LoginDTO $loginDTO);` 因为对象具有明确的定义，也具有解释性；
    - 下下策：`$login->verify($params);` 谁都不知道这个 $params 里面到底有什么；
    - 最下下策：`$login->verify($request->params())`，直接将浏览器输入一股脑全部丢进去，你让后人如何维护？