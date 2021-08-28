# Полезные рецепты для Windows

## Добавить каталог в PATH

Можно решить через создание функции.

```shell
function AddTo-Path{
param(
    [string]$Dir
)

    if( !(Test-Path $Dir) ){
        Write-warning "Supplied directory was not found!"
        return
    }
    $PATH = [Environment]::GetEnvironmentVariable("PATH", "Machine")
    if( $PATH -notlike "*"+$Dir+"*" ){
        [Environment]::SetEnvironmentVariable("PATH", "$PATH;$Dir", "Machine")
    }
}
```

И потом:
```shell
AddTo-Path("C:\Users\user\bin")
```