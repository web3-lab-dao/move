# Packages

Packages cho phép lập trình viên Move tái sử dụng code dễ dàng và chia sẻ chúng
giữa các projects. Move package system cho phép lập trình viên dễ dàng:
* Khởi tạo một package bao gồm Move code;
* Parameterize một package bằng [named addresses](./address.md);
* Import và sử dụng packages trong những Move code khác nhau và khởi tạo named addresses;
* Build packages và generate các artifacts liên quan từ packages
* Làm việc với các interface phổ biến trong khi Move artifacts được biên soạn.

## Package Layout và Manifest Syntax

Một thư mục Move package source bao gồm một `Move.toml` package manifest
file cùng với các thư mục con sau:

```
a_move_package
├── Move.toml      (required)
├── sources        (required)
├── examples       (optional, test & dev mode)
├── scripts        (optional)
├── doc_templates  (optional)
└── tests          (optional, test mode)
```

Những thư mục đánh dấu `required` là _bắt buộc_ phải có để nhận dạng là một Move package và có thể được compiled. Thư mục Optional
là tùy chọn, nếu có thì sẽ chứa các tiến trình tổng hợp. Phụ thuộc vào chế độ package được build (là `test` hay `dev`), Thư mục `tests` và
`examples` cũng sẽ được bao gồm.

Thư mục `sources` có thể bao gồm cả Move modules và Move scripts (cả
transaction scripts và modules đều bao gồm script functions). Thư mục `examples`
có thể chứa code bổ sung được sử dụng cho development và/hoặc
mục đích hướng dẫn, có thể bao gồm khi được compiled bên ngoài `test` hay
`dev` mode.

Thư mục `scripts` được hỗ trợ để transaction scripts có thể phân chia từ
các modules theo mong muốn của package author. Thư mục `scripts`
luôn được bao gồm cho quá trình compilation nếu thư mục này tồn tại.
Tài liệu sẽ được xây dựng bằng các documentation templates có trong thư mục `doc_templates`.

### Move.toml

Move package manifest được định nghĩa trong `Move.toml` file và có thể có cấu trúc như dưới đây. 
Những trường Optional đánh dấu bởi `*`, `+` biểu thị một hoặc vài thành phần:

```
[package]
name = <string>                  # e.g., "MoveStdlib"
version = "<uint>.<uint>.<uint>" # e.g., "0.1.1"
license* = <string>              # e.g., "MIT", "GPL", "Apache 2.0"
authors* = [<string>]            # e.g., ["Joe Smith (joesmith@noemail.com)", "Jane Smith (janesmith@noemail.com)"]

[addresses]  # (Tùy chọn) Định nghĩa named addresses trong package và khởi tạo named addresses trong package graph
# Một hoặc vài dòng code khai báo named addresses với định dạng như dưới đây
<addr_name> = "_" | "<hex_address>" # e.g., std = "_" or my_addr = "0xC0FFEECAFE"

[dependencies] # (Tùy chọn) Đường dẫn tới các dependencies và các instantiations hoặc việc đổi tên các named addresses từ mỗi dependency
# Một hoặc vài dòng code khai báo có định dạng như sau
<string> = { local = <string>, addr_subst* = { (<string> = (<string> | "<hex_address>"))+ } } # local dependencies
<string> = { git = <URL ending in .git>, subdir=<đường dẫn tới thư mục chứa Move.toml trong git repo>, rev=<git commit hash>, addr_subst* = { (<string> = (<string> | "<hex_address>"))+ } } # git dependencies

[dev-addresses] # (Tùy chọn) Tương tự như [addresses], nhưng chỉ được bao gồm trong chế độ "dev" và "test"
# Một hoặc vài dòng code dưới đây định nghĩa dev named addresses
<addr_name> = "_" | "<hex_address>" # e.g., std = "_" or my_addr = "0xC0FFEECAFE"

[dev-dependencies] # (Tùy chọn) Tương tự như [dependencies], nhưng chỉ được bao gồm trong chế độ "dev" and "test"
# Một hoặc vài dòng code dưới đây định nghĩa dev dependencies
<string> = { local = <string>, addr_subst* = { (<string> = (<string> | <address>))+ } }
```

Một ví dụ đơn giản cho package manifest với một local dependency và một git dependency:

```
[package]
name = "AName"
version = "0.0.0"
```

Một ví dụ về package manifest chuẩn bao gồm Move
standard library và biểu diễn named address `Std` với địa chỉ `0x1`:

```
[package]
name = "AName"
version = "0.0.0"
license = "Apache 2.0"

[addresses]
address_to_be_filled_in = "_"
specified_address = "0xB0B"

[dependencies]
# Local dependency
LocalDep = { local = "projects/move-awesomeness", addr_subst = { "std" = "0x1" } }
# Git dependency
MoveStdlib = { git = "https://github.com/diem/diem.git", subdir="language/move-stdlib", rev = "56ab033cc403b489e891424a629e76f643d4fb6b" }

[dev-addresses] # Dùn khi phát triển module này
address_to_be_filled_in = "0x101010101"
```

Hầu hết các thành phần trong package manifest tự bản thân đã giải thích, tuy nhiên named
addresses có thể tương đổi khó hiểu đó ta nên biểu diễn chúng chi tiết một chút

## Named Addresses Trong Quá Trình Compilation

Việc gọi tới Move có [named addresses](./address.md) và
named addresses không thể được định nghĩa trong Move. Bởi vì lí do sau, cho đến khi
named addresses và giá trị của chúng cần được thông qua compiler trong
command line. Với Move package system điều này là không cần thiết, và bạn có thể 
khai báo named addresses trong package, khới tạo named
addresses khác trong phạm vi, và đổi tên named addresses từ các packages khác ở trong
Move package system manifest file. Hãy tiếp tục theo dõi dưới đây:

### Khai báo

Chúng ta có Move module trong `example_pkg/sources/A.move` như dưới đây:

```move
module named_addr::A {
    public fun x(): address { @named_addr }
}
```

Trong `example_pkg/Move.toml` ta có thể khai báo named address `named_addr` bằng 
hai cách. Thứ nhất:

```
[package]
name = "ExamplePkg"
...
[addresses]
named_addr = "_"
```

Khai báo `named_addr` là một named address trong package `ExamplePkg` và
_this address có thể là địa chỉ hợp lệ bất kì value_. Do đó một
package được import có thể lấy giá trị của named address `named_addr` tới bất kì address nào.
Bạn có thể nghĩ tới việc tham số hóa package
`ExamplePkg` bằng named address `named_addr`, và package có thể được
khởi tạo sau bằng cách import package.

`named_addr` được định nghĩa như sau:

```
[package]
name = "ExamplePkg"
...
[addresses]
named_addr = "0xCAFE"
```

Trong đó named address `named_addr` là `0xCAFE` và không thể thay đổi.
Điều này có lợi cho việc import các packages khác, có thể sử dụng named
address mà không cần phải lo về giá trị đã được gán cho nó.

Với hai phương thức khai báo khác nhau, có hai cách để
thông tin về named addresses có thể chạy qua package graph:
* The former ("unassigned named addresses") cho phép các giá trị named address
  từ importation site tới declaration site.
* The latter ("assigned named addresses") cho phép các giá trị named address
  từ the declaration site lên trên package graph tới usage sites.

Với hai phương thức truyền thông tin named address qua
package graph, tiêu chuẩn về phạm vi và cách đổi tên trở nên cần thiết để hiểu.

## Scoping và Cách đổi tên Named Addresses

Một named address `N` trong package `P` thuộc scope nếu:
1. Khai báo một named address `N`; hoặc
2. Một package là một trong `P`'s các dependencies định nghĩa named address
  `N` và có một dependency path trong package graph giữa `P` và
  khai báo package `N` và không được đổi tên `N`.

Ngoài ra, mỗi named address trong package được export. Bởi vì điều này và
scoping rules phía trên, mỗi package có thể được xem như đi cùng với một tập
named addresses mà có thể được đưa tới scope khi package được import,
e.g., Nếu `ExamplePkg` package được import, việc importation có thể đem đến
scope `named_addr` named address. Bởi vì thế, nếu như `P` imports hai
packages `P1` và `P2`, cùng được khai báo một named address `N`, một vấn đề
xuất hiện ở `P`: "`N`" nào có ý nghĩa khi `N` được trỏ tới `P`? Một từ
`P1` hay `P2`? Để ngăn điều này xảy ra trong package, một named
address đến từ đâu, ta có thể bắt buộc một danh sách scopes được cung cấp bởi tất cả
dependencies trong package sẽ được tách rời, và cung cấp _rename named
addresses_ khi package có thể mang vào scope được import.

Thay đổi tên một named address khi import có thể hoàn thành như sau, trong `P`,
`P1`, và `P2` ví dụ:

```
[package]
name = "P"
...
[dependencies]
P1 = { local = "some_path_to_P1", addr_subst = { "P1N" = "N" } }
P2 = { local = "some_path_to_P2"  }
```

Với việc đổi tên `N` trỏ tới `N` từ `P2` và `P1N` trỏ tới `N`
từ `P1`:

```
module N::A {
    public fun x(): address { @P1N }
}
```

Điều này rất quan trọng _renaming không phải local_: khi một named address `N`
được đổi tên thành `N2` trong package `P` tất cả packages import `P` sẽ không
thể thấy `N` mà chỉ có `N2` trừ khi `N` được khai báo lại bên ngoài `P`. Điều này
giải thích tại sao nguyên tắc (2) trong scoping rules tại khởi đầu của section này, chỉ định một
"dependency trong package graph giữa `P` và khai báo
package `N` mà không được đổi tên `N`."

### Cách khởi tạo

Named addresses có thể khởi tạo nhiều lần qua package graph miễn là
nó luôn có cùng giá trị. Có một lỗi xuất hiện nếu cùng named
address (bất kể đổi tên như nào) được khởi tạo với các giá trị khác nhau thông qua
package graph.

Một Move package có thể được compiled nếu toàn bộ named addresses có cùng một giá trị.
Điều này sẽ có một vài vấn đề nếu như package mong muốn không được khởi tạo named
address. Có nghĩa là `[dev-addresses]` được giải quyết. Phần này có thể
đặt các giá trị cho named addresses, nhưng không thể đặt named addresses.
Ngoài ra, chỉ `[dev-addresses]` trong root package được bao gồm trong
`dev` mode. Ví dụ một root package với manifest dưới đây sẽ không được compile
bên ngoài `dev` mode cho nếu như `named_addr` không được khởi tạo:

```
[package]
name = "ExamplePkg"
...
[addresses]
named_addr = "_"

[dev-addresses]
named_addr = "0xC0FFEE"
```

## Usage, Artifacts, và Data Structures

Move package system đi cùng với command line option như làm một thành phần của Move
CLI `move <flags> <command> <command_flags>`. Trừ khi một đường dẫn cụ thể
được cung cấp, tất cả package commands sẽ được chạy tại thời điểm thư mục hoạt động
. Danh sách đầy đủ commands và flags Move CLI có thể tìm thấy bằng `move --help`.

### Cách sử dụng

Một package có thể được compiled qua Move CLI commands, hoặc một thư viện
command trong Rust với function `compile_package`. Điều này sẽ tạo một
`CompiledPackage` chứa compiled bytecode cùng với compilation
artifacts khác (source maps, documentation, ABIs) trong bộ nhớ. `CompiledPackage`
có thể được converted tới một `OnDiskPackage` và ngược lại -- sau đó trở thành dữ liệu của
`CompiledPackage` đặt trong file system với format dưới đây:

```
a_move_package
├── Move.toml
...
└── build
    ├── <dep_pkg_name>
    │   ├── BuildInfo.yaml
    │   ├── bytecode_modules
    │   │   └── *.mv
    │   ├── source_maps
    │   │   └── *.mvsm
    │   ├── bytecode_scripts
    │   │   └── *.mv
    │   ├── abis
    │   │   ├── *.abi
    │   │   └── <module_name>/*.abi
    │   └── sources
    │       └── *.move
    ...
    └── <dep_pkg_name>
        ├── BuildInfo.yaml
        ...
        └── sources
```

Xem `move-package` để biết thêm thông tin về cấu trúc dữ liệu structures và
cách sử dụng Move package system như là một thư viện Rust.
