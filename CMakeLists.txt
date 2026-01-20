cmake_minimum_required(VERSION 3.31)
set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Build configuration")
set(CMAKE_PROJECT_TOP_LEVEL_INCLUDES .devcontainer/cmake/xproinc.cmake)
project(clang-format VERSION 19.1.5 LANGUAGES CXX)
# https://github.com/llvm/llvm-project/releases/tag/llvmorg-19.1.5
set(REPO https://github.com/llvm/llvm-project)
set(url ${REPO}/releases/download/llvmorg-${PROJECT_VERSION}/llvm-project-${PROJECT_VERSION}.src.tar.xz)
set(sha bd8445f554aae33d50d3212a15e993a667c0ad1b694ac1977f3463db3338e542)
set(opt DOWNLOAD_EXTRACT_TIMESTAMP true)
include(FetchContent)
FetchContent_Declare(llvm ${opt} URL ${url} URL_HASH SHA256=${sha} SOURCE_SUBDIR llvm)
set(LLVM_ENABLE_PROJECTS "clang" CACHE STRING "Enable projects")
if(CMAKE_SYSTEM_PROCESSOR MATCHES "^(x86_64|amd64|AMD64)$")
  set(LLVM_TARGETS_TO_BUILD "X86" CACHE STRING "Targets to build")
elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "^(aarch64|arm64|ARM64)$")
  set(LLVM_TARGETS_TO_BUILD "AArch64" CACHE STRING "Targets to build")
endif()
FetchContent_MakeAvailable(llvm)
xpExternPackage(EXE_PATH ${CMAKE_INSTALL_BINDIR}/clang-format
  BASE v0 XPDIFF "fetch"
  WEB "https://clang.llvm.org/docs/ClangFormat.html" UPSTREAM "github.com/llvm/llvm-project"
  DESC "used to format C/C++/Java/JavaScript/JSON/Objective-C/Protobuf/C# code (clang/tools/clang-format in upstream repo)"
  LICENSE "[Apache-2.0](https://releases.llvm.org/11.0.0/LICENSE.TXT 'Apache License v2.0 with LLVM Exceptions, see https://clang.llvm.org/features.html#license and https://llvm.org/docs/DeveloperPolicy.html#copyright-license-and-patents')"
  )
install(TARGETS clang-format RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
