# webpack

## Webpack Resolve

* Webpack 의 **모듈 번들링** 관점에서 봤을 때, 모듈 간의 의존성을 고려하여 모듈을 로딩해야 한다.
* 따라서, 모듈을 어떤 위치에서 어떻게 로딩할지에 관해 정의를 하는 것이 바로 Module Resolution

``` js
// ES6
import foo from 'path/to/module'
// ES5
require('path/to/module');
```

***모듈을 어렇게 로딩해오느냐?***

### 1. 절대경로를 이용한 파일로딩

* 파일의 경로를 모두 입력

``` js
import "/home/me/file";
import "C:\\Users\\me\\file";
```

### 2. 상대경로를 이용한 파일로딩

* 해당 모듈이 로딩되는 시점 위치에 기반하여, 상대 경로를 절대 경로로 인식하여 로딩한다.

``` js
import "../src/file1";
import "./file2";
```

## Resolve Option

* config파일에 `resolve`를 추가하여 모듈 로딩에 관련된 옵션 사용

**`alias`**
  * 특정 모듈을 로딩할때 `alias` 옵션을 이용하여 별칭으로 더 쉽게 로등이 가능

``` js
alias: {
  Utilties: path.resolve(__dirname, 'src/path/utilties/')
}

import Utility from '../../src/path/utilties/utility'; // ..?
// alias 사용시 '/src/path/utilties' 대신 'Utilties' 활용
import Utility from 'Utilities/utility';
```
