#### RU: Это инструкция для создание статической линковки(сборки) для QT 6.2.0, с помощью данной инструкции вы сможете делать portable версии своих проектов.

Внимание! Все параметры выставлены по ПРИМЕРУ, у вас значение полей может быть другое!

## Необходимые компоненты

- Online installer(https://www.qt.io/download-qt-installer?hsCtaTracking=99d9dd4f-5681-48d2-b096-470725510d34%7C074ddad0-fdef-4e53-8aa8-5e8a876d6ab4)
- Visual Studio 2019(https://visualstudio.microsoft.com/ru/downloads/)
- Исходники Qt
- Strawberry Perl 5+ (https://strawberryperl.com/)
- Python 3.5+ (https://www.python.org/)
- CMake 3.25+ (https://cmake.org/download/)
- Ninja (https://github.com/ninja-build/ninja/releases)

## Установка необходимых компонентов

1) Установка QT допустима с параметрами которые вам требуются, обязательно должны быть установлены исходники QT(Sources)
2) Установка Visual Studio 2019 допустима с параметрами которые вам требуются
3) Установка Strawberry Perl желательна с настройками по умолчанию
4) Установка Python желательна с настройками по умолчанию(галочка Add to PATH, должна быть включена)
5) Установка CMake желательна с настройками по умолчанию
6) Ninja скачивается как архив и распаковывается в удобное вам место, у меня это "C:\ninja". После распаковки там должен быть только исполняемый файл "ninja.exe"

## Проверка расположения необходимых компонентов

Проверять обязательно в CMD запущенной от имени администратора, в скобках команды которые нужно вызывать!
К проверке обязательны:

1) Strawberry Perl (where perl.exe)
2) Python (where python.exe)
3) CMake (where cmake.exe)
4) Ninja (where ninja.exe)

В случае если выдаёт "ИНФОРМАЦИЯ: не удается найти файлы по заданным шаблонам.", то нужно вручную указать путь для нужного компонента в CMD

```shell script
set "<ИМЯ КОМПОНЕНТА>_ROOT=<ПУТЬ ДО ИСПОЛНЯЕМЫХ ФАЙЛОВ КОМПОНЕНТА>"
set PATH=%<ИМЯ КОМПОНЕНТА>_ROOT%;%PATH%
```
Пример:
```shell script
set "CMAKE_ROOT=C:\Program Files\CMake\bin"
set PATH=%CMAKE_ROOT%;%PATH%
```

После проделанной процедуры вызовите команду where для требуемого компонента, ошибка должна пропасть!

## Порядок установки и сборки

1. Скачиваем и устанавливаем необходимые компоненты, проверяя их командой where, в случае ошибок делаем процедуры описаные выше!
2. Скачиваем QT Online Installer и запускаем. Проходим авторизацию и другие этапы пока не попадаем в окно выбора компонентов!
3. Выбираем версию Qt 6.2.0, раскрываем пункт и ставим галочки:
- [x] MSVC 2019 64-bit
- [x] Sources
- [x] Qt Quick 3D
- [x] Qt 5 Compatibility Module
- [x] Qt Shader Tools
- [x] Additional Libraries
- [x] Qt Debug Information Files
- [x] Qt Quick Timeline

Спускаемся ниже где расположен Developer and Designer Tools, раскрываем пункт:

- [x] Qt Creator X.X.X
- [x] Qt Creator X.X.X CDB Debugger Support
- [x] Debugging Tools for Windows
Убираем галочки на CMake и Ninja, так как мы их установили ранее.
Внимание! Такие настройки ставил я, вы можете убирать галочки или включать новые!
Ждём окончания установки! 
4. Устанавливаем Visual Studio(если он не установлен), тут главное выбрать SDK который совместим под вашу версию QT и средства компиляции!
5. Открываем x64 Native Tools Command Prompt for VS 2019
5.1 Переходим в директорию с установленным QT (в моём случае это D:\QT\6.2.0
   
   Если на диске C:
   ```shell script
   cd C:\QT\6.2.0\
   ```
   
   Если на диске D:
   ```shell script
   cd /d D:\QT\6.2.0\
   ```
   
5.2 Создаем папку для сборки

   ```shell script
   mkdir build
   cd build
   ```
6. Запускаем конфигурационный скрипт

   ```shell script
   <Путь до исходников Qt>\configure.bat -debug-and-release -static -static-runtime -opensource -confirm-license -platform win32-msvc -qt-zlib -qt-libpng -qt-libjpeg -nomake examples -nomake tests -no-opengl -skip qtscript -skip qtquick3d -prefix "<Путь куда будет собираться статическая версия>"
   ```
   - `<Путь до исходников Qt>` - как правило находится в папке с установленным QT, в моём случае: D:\QT\6.2.0\Src
   - `<Путь куда будет собираться статическая версия>` - в моём случае это папка с установленным Qt D:\QT\6.2.0\MSCV-Static
   # "<" и ">" не вписываем, должно получится вот так(это пример):
   ```shell script
   D:\QT\6.2.0\Src\configure.bat -debug-and-release -static -static-runtime -opensource -confirm-license -platform win32-msvc -qt-zlib -qt-libpng -qt-libjpeg -nomake examples -nomake tests -no-opengl -skip qtscript -skip qtquick3d -prefix "D:\QT\6.2.0\MSCV-Static"
   ```

   Ключи:
   - `skip <module>` - исключает из процесса сборки отдельный подмодуль
   - `nomake examples` - исключает из процесса сборки примеры программ
   - `nomake tests` - исключает из процесса сборки тесты
   - `platform <platform>` - определяет платформу, для которой будет собран Qt, в данном случае Windows с MSVC
   - `no-opengl` - не использовать OpenGL для отрисовки интерфейса
   - `qt-zlib`, `qt-libpng`, `qt-libjpeg` - использовать библиотеки `zlib`, `libpng`, `libjpeg` поставляемые вместе с Qt
   - `opensource` - использовать open source вариант Qt
   - `confirm-license` - автоматически принять лицензию Qt
   - `debug-and-release`, `release`, `debug` - варианты сборок
   - `static`, `static-runtime` - включить статическую компоновку Qt и runtime
   - `prefix <prefix>` - путь до папки, в которую будут скопированы откомпилированные файлы Qt
6.1 Ждём окончания процесса! После этого вконце будет написан результат, без ошибок он выглядит так:
Qt is now configured for building. Just run 'cmake --build . --parallel'

``"Once everything is built, you must run 'ninja install'
Qt will be installed into 'D:/QT/6.2.0/MSCV-Static'

To configure and build other Qt modules, you can use the following convenience script:
        D:/QT/6.2.0/MSCV-Static/bin/qt-configure-module.bat

If reconfiguration fails for some reason, try to remove 'CMakeCache.txt' from the build directory

-- Configuring done (306.1s)
-- Generating done (22.6s)
CMake Warning:
  Manually-specified variables were not used by the project:

    BUILD_qtscript


-- Build files have been written to: D:/QT/6.2.0/build"``

6.2 Воспользуйтесь переводчиком если что либо из написаного выше не понятно!
7. QT настроен для сборки, запускаем команду 
```shell script
   cmake --build . --parallel
   ```
7.1 Ждём окончания настроки(может занять много времени, зависит от ЦП и скорости записи)
8. После окончания 7.1 запускаем команду 
```shell script
   ninja install
   ```
8.1 После окончания сборки, можем перейти в QT для настройки профиля и комплекта!
8.2 Вывод консоли должен быть такой:
```
[11790/11790] Linking CXX static library qtbase\qml\QtWebView\qtwebviewquickplugind.lib
```
