---
id: overview
title: Overview
sidebar_label: Move
---

Move là một ngôn ngữ lập trình thế hệ mới, với các tính năng bảo mật, sandboxed, và đã chính thức được công nhận. Move lần đầu được sử dụng trong Diem blockchain, cung cấp nền tảng để thực thực thi Diem. Tuy nhiên Move cũng đã được phát triển cho những mục đích bên ngoài lĩnh vực blockchain.

### Bắt đầu tại đây

<CardsWrapper>
  <SimpleTextCard
    icon="img/introduction-to-move.svg"
    iconDark="img/introduction-to-move-dark.svg"
    overlay="Hiểu về nền tảng của Move, trạng thái hiện tại và kiến trúc"
    title="Giới thiệu"
    to="/docs/move/move-introduction"
  />
  <SimpleTextCard
    icon="img/modules-and-scripts.svg"
    iconDark="img/modules-and-scripts-dark.svg"
    overlay="Hiểu các loại chương trình của Move: Modules và Scripts"
    title="Modules và Scripts"
    to="/docs/move/move-modules-and-scripts"
  />
  <SimpleTextCard
    icon="img/placeholder.svg"
    iconDark="img/placeholder-dark.svg"
    overlay="Làm quen với Move bằng cách tạo các đồng coin"
    title="Tutorial đầu tiên: Tạo Coins"
    to="/docs/move/move-tutorial-creating-coins"
  />
</CardsWrapper>

### Kiểu dữ liệu nguyên thủy

<CardsWrapper>
  <SimpleTextCard
    icon="img/integers-bool.svg"
    iconDark="img/integers-bool-dark.svg"
    overlay="Move hỗ trợ 3 kiểu unsigned integer: u8, u64, and u128"
    title="Integers"
    to="/docs/move/move-integers"
  />
  <SimpleTextCard
    icon="img/integers-bool.svg"
    iconDark="img/integers-bool-dark.svg"
    overlay="bool là kiểu dữ liệu nguyên thủy true và false"
    title="Bool"
    to="/docs/move/move-bool"
  />
  <SimpleTextCard
    icon="img/address.svg"
    iconDark="img/address-dark.svg"
    overlay="address là kiểu dữ liệu có sẵn trong Move dùng để chỉ ra địa chỉ trong global storage"
    title="Address"
    to="/docs/move/move-address"
  />
  <SimpleTextCard
    icon="img/vector.svg"
    iconDark="img/vector-dark.svg"
    overlay="vector<T> là kiểu dữ liệu collection nguyên thủy duy nhất cung cấp bởi Move"
    title="Vector"
    to="/docs/move/move-vector"
  />
  <SimpleTextCard
    icon="img/signer.svg"
    iconDark="img/signer-dark.svg"
    overlay="signer là kiểu resource có sẵn trong Move. signer cho phép holder tương tác thay mặt cho address cụ thể"
    title="Signer"
    to="/docs/move/move-signer"
  />
  <SimpleTextCard
    icon="img/move-references.svg"
    iconDark="img/move-references-dark.svg"
    overlay="Move có 2 kiểu references: immutable & và mutable &mut"
    title="References"
    to="/docs/move/move-references"
  />
  <SimpleTextCard
    icon="img/tuples.svg"
    iconDark="img/tuples-dark.svg"
    overlay="Để hỗ trợ trả về nhiều kiểu dữ liệu, Move sử dụng tuple-like expressions. Chúng ta có thể dùng unit() để trả về tuple rỗng"
    title="Tuples và Unit"
    to="/docs/move/move-tuples-and-unit"
  />
</CardsWrapper>

### Khái niệm cơ bản

<CardsWrapper>
  <SimpleTextCard
    icon="img/local-variables-and-scopes.svg"
    iconDark="img/local-variables-and-scopes-dark.svg"
    overlay="Biến cục bộ trong Move tương đồng với những từ vựng cơ bản"
    title="Local Variables và Scopes"
    to="/docs/move/move-variables"
  />
  <SimpleTextCard
    icon="img/abort-and-return.svg"
    iconDark="img/abort-and-return-dark.svg"
    overlay="return và abort là hai cấu trúc luồng xử lí để kết thúc một tiến trình, một dành cho phương thức hiện tại và một là dành cho toàn bộ tiến trình xử lí"
    title="Abort & Assert"
    to="/docs/move/move-abort-and-assert"
  />
  <SimpleTextCard
    icon="img/conditionals.svg"
    iconDark="img/conditionals-dark.svg"
    overlay="Câu điều kiện if chỉ định các dòng code sẽ được thực thi khi điều kiện if là true"
    title="Điều kiện"
    to="/docs/move/move-conditionals"
  />
  <SimpleTextCard
    icon="img/loops.svg"
    iconDark="img/loops-dark.svg"
    overlay="Move có hai cấu trúc cho vòng lặp: while và loop"
    title="While và Loop"
    to="/docs/move/move-while-and-loop"
  />
  <SimpleTextCard
    icon="img/functions.svg"
    iconDark="img/functions-dark.svg"
    overlay="Cú pháp về hàm trong Move được chia sẻ giữa các module functions và script functions"
    title="Functions"
    to="/docs/move/move-functions"
  />
  <SimpleTextCard
    icon="img/structs-and-resources.svg"
    iconDark="img/structs-and-resources-dark.svg"
    overlay="Struct là cấu trúc dữ liệu thân thiện với người dùng, bao gồm các field với kiểu dữ liệu. Resource là một kiểu struct mà không thể bị copy và drop"
    title="Structs và Resources"
    to="/docs/move/move-structs-and-resources"
  />
  <SimpleTextCard
    icon="img/constants.svg"
    iconDark="img/constants-dark.svg"
    overlay="Constants are a way of giving a name to shared, static values inside of a module or script"
    title="Constants"
    to="/docs/move/move-constants"
  />
  <SimpleTextCard
    icon="img/generics.svg"
    iconDark="img/generics-dark.svg"
    overlay="Generics có thể sử dụng để định nghĩa các function và structs với các kiểu dữ liệu đầu vào khác nhau"
    title="Generics"
    to="/docs/move/move-generics"
  />
  <SimpleTextCard
    icon="img/equality.svg"
    iconDark="img/equality-dark.svg"
    overlay="Move hỗ trợ hai kiểu so sánh == và !="
    title="So sánh bằng"
    to="/docs/move/move-equality"
  />
  <SimpleTextCard
    icon="img/uses-and-aliases.svg"
    iconDark="img/uses-and-aliases-dark.svg"
    overlay="Cú pháp use có thể được dùng để tạo ra aliases cho các thành phần trong các modules khác nhau"
    title="Uses & Aliases"
    to="/docs/move/move-uses-and-aliases"
  />
</CardsWrapper>

### Global Storage

<CardsWrapper>
  <SimpleTextCard
    icon="img/intro-to-global-storage.svg"
    iconDark="img/intro-to-global-storage-dark.svg"
    overlay="Mục đích của Move là đọc và ghi tới global storage"
    title="Global Storage Structure"
    to="/docs/move/move-global-storage-structure"
  />
  <SimpleTextCard
    icon="img/intro-to-global-storage.svg"
    iconDark="img/intro-to-global-storage-dark.svg"
    overlay="Move có thể thêm, sửa, xóa resources trong global storage, sử dụng 5 cấu trúc"
    title="Global Storage Operators"
    to="/docs/move/move-global-storage-operators"
  />
</CardsWrapper>

### Tham khảo

<CardsWrapper>
  <SimpleTextCard
    icon="img/standard-library.svg"
    iconDark="img/standard-library-dark.svg"
    overlay="Thư viện Move căn bản đưa ra các interfaces thực hiện các chức năng vectors, kiểu dữ liệu tùy chọn, error codes và fixed-point numbers"
    title="Thư viện chuẩn"
    to="/docs/move/move-standard-library"
  />
  <SimpleTextCard
    icon="img/coding-conventions.svg"
    iconDark="img/coding-conventions-dark.svg"
    overlay="Conventions khi viết code Move"
    title="Coding Conventions"
    to="/docs/move/move-coding-conventions"
  />
</CardsWrapper>
