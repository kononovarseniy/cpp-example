# Устанавливаем диапазон версий CMake с которыми можем работать.
# Более старые версии не будут работать с нашим проектом.
# Более новые будут симулировать поведение старых
# устанавливая политики (базовые настройки поведения) в старое состояние.
cmake_minimum_required(VERSION 3.23...3.31)

# Имя проекта и базовая информация о нем.
project(cpp_example CXX)

# Сообщаем какой стандарт языка будем использовать.
set(CMAKE_CXX_STANDARD 20)
# Говорим, что вообще никак без него не можем.
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Создаем исполняемый таргет.
add_executable(hello_world)

# Добавляем исходные файлы в таргет.
target_sources(hello_world
    PUBLIC
    FILE_SET HEADERS
    BASE_DIRS include
    FILES
        # Тут указываем заголовочные файлы. Если их нет то все ключевые слова выше можно пропустить.
        # На самом деле можно и просто перечислить вообще все файлы без каких либо ключевых слов.
        # Но их использование позволяет просто писать команды install.
    PRIVATE
        # Тут все остальные исходныфе файлы таргета.
        src/main.cpp
)

# Включаем как можно больше предупреждений и считаем их ошибками.
target_compile_options(hello_world
    PRIVATE
        # Для MSVC.
        $<$<CXX_COMPILER_ID:MSVC>:/W4 /WX>
        # Для GCC, MinGW и clang.
        $<$<NOT:$<CXX_COMPILER_ID:MSVC>>:-Wall -Wextra -Wpedantic -Werror>
)

# []
# Установка в систему или в пакет.
# По сути копирование исполняемых файлов, заголовочных файлов библиотек и самих библиотек.
# Мы устанавливать не будет, поэтому приводим только для кругозора.
install(TARGETS hello_world
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
    FILE_SET HEADERS DESTINATION include
)
