
# bash notes - id, color, heredoc

---

## How can I find my User ID (UID) from terminal?

```shell=
$ id -u <user name>
or 
$ echo $UID
```

---

## list bash declared functions

```shell=
$ declare -f
```

---

## print color sting

[ANSI_escape_code](https://en.wikipedia.org/wiki/ANSI_escape_code)
ANSI escape color codes
```shell=
Black        0;30     Dark Gray     1;30
Red          0;31     Light Red     1;31
Green        0;32     Light Green   1;32
Brown/Orange 0;33     Yellow        1;33
Blue         0;34     Light Blue    1;34
Purple       0;35     Light Purple  1;35
Cyan         0;36     Light Cyan    1;36
Light Gray   0;37     White         1;37
```

```shell=
RED='\033[0;31m'
NC='\033[0m' # No Color
printf "I ${RED}love${NC} Stack Overflow\n"
```

---

## heredoc 

**EOF or 'EOF'**

```shell=
$ XYZ=pqr
$ cat <<EOF
> echo $XYZ
> EOF
echo pqr
$ cat <<'EOF'
> echo $XYZ
> EOF
echo $XYZ
```

---
###### tags: `bash` `linux`
