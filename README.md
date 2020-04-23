# Quaternion library
## 【日本語】
  Rustで作成した四元数（クォータニオン）計算用のライブラリです。

  関数の動作については、ドキュメンテーションコメントを参照してください。

  また、四元数自体について詳しく知りたい方は、以下のページをご覧ください。

  * [四元数まとめ資料を書いた（宇宙電波実験室）](https://space-denpa.jp/2019/03/26/quaternion-doc/)

### 使用時の注意点
不要なオーバーヘッドを減らすため、基本的に関数の内部では正規化を行っていません。そのため、引数がVersor（単位四元数）である関数を使用する際には、引数のノルムが1となるようにしてください。

ただし、DCMをVersorに変換する関数や近似式を使う関数（lerpなど）では、出力がVersorになるように関数内部で正規化を行っています。

### 例外処理に関して
 零ベクトルを入力した場合には例外処理を行いますが、すべての要素が零となる四元数が入力される状況（ベクトルを純虚四元数として扱った場合などに起こる）は考慮していません。

## 【English】
  It's a Quaternion library written in Rust.

  Refer to the documentation comments for function behavior.

### Precautions when using
To reduce unnecessary overhead, normalization is basically not performed inside the function. Therefore, when using a function whose argument is Versor (unit quaternion), make sure that the norm of the argument is 1.

However, in functions that convert DCM to Versor and functions that use approximations (such as lerp), normalization is performed inside the function so that the output is Versor.

### About exception handling
When a zero vector is input, exception processing is performed. However, a situation where a quaternion in which all elements are zero (which occurs when a vector is treated as a pure quaternion) is not considered.

# Example of use
#### Cargo.toml
```
[dependencies]
quaternion = {git = "https://github.com/HamaguRe/quaternion.git", version = "2.0"}
```

#### src/main.rs
```
use quaternion as quat;
use quat::Vector3;

const PI: f64 = std::f64::consts::PI;
const EPSILON: f64 = 1e-14;

fn main() {
    // Position vector
    let r: Vector3<f64> = [2.0, 2.0, 0.0];

    // Generates a quaternion representing the
    // rotation of π/2[rad] around the y-axis.
    let q = quat::from_axis_angle([0.0, 1.0, 0.0], PI/2.0);

    let result = quat::vector_rotation(q, r);

    // Check if the calculation is correct.
    let diff = quat::sub_vec(result, [0.0, 2.0, -2.0]);
    for i in 0..3 {
        assert!( diff[i].abs() < EPSILON );
    }
}
```