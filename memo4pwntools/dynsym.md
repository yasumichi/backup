Pwntoolsの`dynsym`（ダイナミックシンボル）機能は、主に動的リンクされたバイナリ（ELFファイル）内の関数やシンボルのアドレスを、実行時またはローカル解析時に解決・特定するために使用されます。特に`pwnlib.dynelf` モジュールは、メモリリーク（memleak）や情報漏洩（infoleak）を利用して、リモートプロセス上のlibc関数などを特定する際に強力です。

- [pwnlib.dynelf — Resolving remote functions using leaks — pwntools 4.15.0 documentation](https://docs.pwntools.com/en/stable/dynelf.html)

## 主な利用機能と用途

1. pwnlib.dynelf.DynELF:
  - 用途: リモートプロセスで、すでに解けている（またはメモリリークで取得した）ポインタを基に、libcなどの共有ライブラリ内のシンボル（system, printfなど）の絶対アドレスを解析します。
  - 方法: leak関数（メモリを読み出す関数）と、バイナリ内のポインタ（例：GOTテーブルの指すアドレス）をDynELFに渡して利用します。
  - 特徴: find_base()でライブラリのベースアドレスを特定したり、lookup()で特定の関数アドレスを取得したりできます。
1. ELF.symbols (動的解析):
  - 用途: ローカルのELFファイルを解析し、シンボル名からオフセットやアドレスを高速に取得します。
  - 例: elf.symbols['system']。
1. Ret2dlresolve (応用テクニック):
  - 用途: pwnlib.rop.ret2dlresolveは、PLT/GOTが不完全な場合や、標準のlibcが使用できない環境で、_dl_runtime_resolveを悪用して任意関数を呼び出す攻撃手法です。
  - 機能: Ret2dlresolvePayloadを使用して、必要なシンボル（.dynsym）や文字列（.dynstr）の構造を自動生成し、ROPチェーンに組み込みます。

### 関連リンク

- [pwntools-tutorial/elf.md at master · Gallopsled/pwntools-tutorial](https://github.com/Gallopsled/pwntools-tutorial/blob/master/elf.md)
- [Ret2dlresolve should check that VERSYM[ ELF32_R_SYM (reloc->r_info) ] is valid · Issue #2670 · Gallopsled/pwntools](https://github.com/Gallopsled/pwntools/issues/2670)

## 実装・使用例

```python
from pwn import *

# 1. ローカルELFファイルの解析
e = ELF('./target_binary')
print(hex(e.symbols['main']))
print(hex(e.got['puts']))

# 2. リモートでのDynELF使用例
# leakerはメモリを読み出す関数 (例: p.sendline(...); p.recv(...))
def leak(addr):
    # 実際はここで脆弱性を使ってメモリリークさせる
    data = r.read(addr, 4)
    return data

# pointerは、ターゲットライブラリ（libc）内の既知の有効なアドレス
d = DynELF(leak, pointer=0x7ffff7a0d000)
system_addr = d.lookup('system', 'libc')
print(f"System address: {hex(system_addr)}")
```

## 関連・補足情報

- DYNSYMと関係するセクション: .dynsym（ダイナミックシンボルテーブル）、.dynstr（シンボル名文字列）、.hash（高速検索用ハッシュテーブル）。
- libc-databaseとの連携: DynELFは、リークしたBuild IDを使用して、libcdb（libc-database）から自動的に正しいlibcファイルをロードすることも可能です。

`dynsym` 機能の活用により、シンボルが削除された（stripped）バイナリでも、動的解析を通じてシンボルの特定が可能になります。

