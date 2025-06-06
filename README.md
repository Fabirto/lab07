export GITHUB_USERNAME=<имя_пользователя>
$ alias gsed=sed
  создает переменную
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
активирует скрипт


$ git clone https://github.com/${GITHUB_USERNAME}/lab06 projects/lab07
$ cd projects/lab07
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab07
клонирует репозиторий и меняет оригин

$ mkdir -p cmake
$ wget https://raw.githubusercontent.com/cpp-pm/gate/master/cmake/HunterGate.cmake -O cmake/HunterGate.cmake
$ gsed -i '/cmake_minimum_required(VERSION 3.4)/a\

include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/cpp-pm/hunter/archive/v0.23.251.tar.gz"
    SHA1 "5659b15dc0884d4b03dbd95710e6a1fa0fc3258d"
)
' CMakeLists.txt
создает директорию cmake, скачивает huntergate, добавляет строки кода в cmakelists


$ git rm -rf third-party/gtest
$ gsed -i '/set(PRINT_VERSION_STRING "v\${PRINT_VERSION}")/a\

hunter_add_package(GTest)
find_package(GTest CONFIG REQUIRED)
' CMakeLists.txt
$ gsed -i 's/add_subdirectory(third-party/gtest)//' CMakeLists.txt
$ gsed -i 's/gtest_main/GTest::main/' CMakeLists.txt
удаляет gtest,удаляет строки, добавляет строки в cmakelists

$ cmake -H. -B_builds -DBUILD_TESTS=ON
$ cmake --build _builds
$ cmake --build _builds --target test
$ ls -la $HOME/.hunter
собирает проект, делает тесты, показывает зависимости hunter


$ git clone https://github.com/cpp-pm/hunter $HOME/projects/hunter
$ export HUNTER_ROOT=$HOME/projects/hunter
$ rm -rf _builds
$ cmake -H. -B_builds -DBUILD_TESTS=ON
$ cmake --build _builds
$ cmake --build _builds --target test
настройка для работы с hunter, сборка и тестирование

$ cat $HUNTER_ROOT/cmake/configs/default.cmake | grep GTest
$ cat $HUNTER_ROOT/cmake/projects/GTest/hunter.cmake
$ mkdir cmake/Hunter
показывает настройки gtest, выводит содержимое hunter, создает папку

$ cat > cmake/Hunter/config.cmake <<EOF
hunter_config(GTest VERSION 1.7.0-hunter-9)
EOF
# add LOCAL in HunterGate function
создает файл

$ mkdir demo
$ cat > demo/main.cpp <<EOF
#include <print.hpp>

#include <cstdlib>

int main(int argc, char* argv[])
{
  const char* log_path = std::getenv("LOG_PATH");
  if (log_path == nullptr)
  {
    std::cerr << "undefined environment variable: LOG_PATH" << std::endl;
    return 1;
  }
  std::string text;
  while (std::cin >> text)
  {
    std::ofstream out{log_path, std::ios_base::app};
    print(text, out);
    out << std::endl;
  }
}
EOF
создает директорию и файл

$ gsed -i '/endif()/a\

add_executable(demo ${CMAKE_CURRENT_SOURCE_DIR}/demo/main.cpp)
target_link_libraries(demo print)
install(TARGETS demo RUNTIME DESTINATION bin)
' CMakeLists.txt
вставляет строки в cmakelists


$ mkdir tools
$ git submodule add https://github.com/ruslo/polly tools/polly
$ tools/polly/bin/polly.py --test
$ tools/polly/bin/polly.py --install
$ tools/polly/bin/polly.py --toolchain clang-cxx14
добавляет polly, настраивает окружение, генерирует проект

Report

$ popd
$ export LAB_NUMBER=07
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
