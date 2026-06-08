#Заголовок первого уровня
##Заголовок второго уровня
###Заголовок третьего уровня
####Заголовок четвертого уровня

*курсив* или _курсив_
**жирный** или __жирный__
***жирный курсив*** или ___жирный курсив___
~~зачеркунутый текст~~

- элемент 1
- элемент 2
    - вложенный элемент (2 пробел или табуляция)
  - вложенный элемент (2 пробел или табуляция)
    
1. Первый 
2. Второй
3. Третий
  
[текст ссылки](https://ru.meming.world/images/ru/thumb/7/73/%D0%A8%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD_%D0%BA%D0%BE%D1%82.jpg/300px-%D0%A8%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD_%D0%BA%D0%BE%D1%82.jpg)

![альтернативный текст](https://ru.meming.world/images/ru/thumb/7/73/%D0%A8%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD_%D0%BA%D0%BE%D1%82.jpg/300px-%D0%A8%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD_%D0%BA%D0%BE%D1%82.jpg)

`Write-Host`

```bash
<#
.SYNOPSIS
    Автоматическая сотировка файлов в папке Загрузки.
.DESCRIPTION
    Создаёт подпапки по типу и перемещает в них содержимое.
    Безопасно для повторного запуска
.NOTES
    Версия 0.01
#>

[CmdletBinding()]

param(
    [switch]$DryRun
)

$DownloadPath = "$env:USERPROFILE\Downloads"

$Categories =@{
    "Images" = @("*.jpg", "*.jpeg")
    "Documents" = @("*.doc", "*.docx", "*.pdf", "*.txt")
    "Archive" = @("*.zip", "*.rar")
    "Installers" = @("*.exe", "*.bat", "*.ps1")
    "Media" = @("*.mp3", "*.avi")
}
Write-Host "Начало сортировки" -ForegroundColor Cyan
Write-Host "Папка: $DownloadPath" -ForegroundColor Gray

foreach ($Category in $Categories.Keys) {
    $TargetFolder = Join-Path $DownloadPath $Category

    if (-not(Test-Path $TargetFolder)){
        New-Item -Path $TargetFolder -ItemType Directory -Force | Out-Null
        Write-Host "Создана папка: $Category" -ForegroundColor DarkGray
    }


    foreach ($Pattern in $Categories[$Category]){


    $File = Get-ChildItem -Path $DownloadPath -Filter $Pattern -File -ErrorAction SilentlyContinue

    foreach ($File in $Files) {
        if ($File.DirectoryName -eq $TargetFolder) { continue }

        $Dest = Join-Path $TargetFolder $File.Name

        if (Test-Path $Dest) {
            $Base = [System.IO.Path]::GetFileNameWithoutExtension($File.Name)
            $Exit = [System.IO.Path]::GetExtension($File.Name)
            $Timestamp = Get-Date -Format "yyyyMMdd_HHmmss"
            $Dest = Join-Path $TargetFolder "${Base}_$Timestamp$Exit"
            Write-Host "Дубликат имени: $($File.Name) -> переименован" -ForegroundColor Yellow

            }

            if ($DryRun) {
                Write-Host "[ПРОГНОЗ] ($File.Name) -> $Category\" -ForegroundColor Yellow 
            } else{
                try {
                    Move-Item -Path $File.FullName -Destination $Dest -Forse -ErrorAction Stop

                    Write-Host "ок $($File.Name) -> $Category\" -ForegroundColor DarkGreen
                    }
                    catch{
                    Write-Host "не удалось переместить $($File.Name):$($_.Exeption.Message)"
                    }
                    }
                    }
    }
}


Write-Host "Сортировка завершена" -ForegroundColor Cyan
```
> это цитата
> она может занимать несколько строк

---

***

| Имя | Возраст | Город | 
|-----|---------|-------|
| Антон | 20    | Санккт-Петербург|
| Иван | 25 | Москва |