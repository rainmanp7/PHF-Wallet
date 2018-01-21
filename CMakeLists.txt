cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)


project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB cryptonote)


include_directories(${CMAKE_BINARY_DIR}
  src
  include
  ${CRYPTONOTE_LIB}/cryptonote/src/Common
  ${CRYPTONOTE_LIB}/cryptonote
  ${CRYPTONOTE_LIB}/external
  ${CRYPTONOTE_LIB}/include
  ${CRYPTONOTE_LIB}/src/crypto
  ${CRYPTONOTE_LIB}/src/CryptoNoteCore
  ${CRYPTONOTE_LIB}/src/Serialization
  ${CRYPTONOTE_LIB}/src)

message(STATUS "my_gui_sources = ${my_gui_sources}")


find_package(Qt5Core REQUIRED)
find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)
find_package(Qt5Network REQUIRED)
find_package(Qt5DBus REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "")
configure_file("${CRYPTONOTE_LIB}/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DVERSION=\"${CN_VERSION}\")
add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")


if(POLICY CMP0071)
  cmake_policy(SET CMP0071 NEW)
endif()

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CRYPTONOTE_SOURCES


file(GLOB_RECURSE CRYPTONOTE_SOURCES
 ${CRYPTONOTE_LIB}/cryptonote/external/miniupnpc/connecthostport.c
 ${CRYPTONOTE_LIB}/cryptonote/external/miniupnpc/igd_desc_parse.c
 ${CRYPTONOTE_LIB}/cryptonote/external/miniupnpc/minisoap.c
   ${CRYPTONOTE_LIB}/cryptonote/external/miniupnpc/miniupnpc.c
   ${CRYPTONOTE_LIB}/cryptonote/external/miniupnpc/miniwget.c
   ${CRYPTONOTE_LIB}/cryptonote/external/miniupnpc/minixml.c
   ${CRYPTONOTE_LIB}/cryptonote/external/miniupnpc/portlistingparse.c
   ${CRYPTONOTE_LIB}/cryptonote/external/miniupnpc/receivedata.c
   ${CRYPTONOTE_LIB}/cryptonote/external/miniupnpc/upnpcommands.c
   ${CRYPTONOTE_LIB}/cryptonote/external/miniupnpc/upnpreplyparse.c
   ${CRYPTONOTE_LIB}/cryptonote/src/Common/Base58.cpp
   ${CRYPTONOTE_LIB}/cryptonote/src/Common/CommandLine.cpp
   ${CRYPTONOTE_LIB}/cryptonote/src/Common/Util.cpp
   ${CRYPTONOTE_LIB}/cryptonote/src/Common/StringTools.cpp
   ${CRYPTONOTE_LIB}/cryptonote/src/Common/JsonValue.cpp
   ${CRYPTONOTE_LIB}/cryptonote/src/Common/ConsoleTools.cpp
   ${CRYPTONOTE_LIB}/cryptonote/src/Common/MemoryInputStream.cpp
   ${CRYPTONOTE_LIB}/cryptonote/src/Common/PathTools.cpp
   ${CRYPTONOTE_LIB}/cryptonote/src/Common/StdInputStream.cpp
   ${CRYPTONOTE_LIB}/cryptonote/src/Common/StdOutputStream.cpp
   ${CRYPTONOTE_LIB}/cryptonote/src/Common/StreamTools.cpp
   ${CRYPTONOTE_LIB}/cryptonote/src/Common/StringOutputStream.cpp
   ${CRYPTONOTE_LIB}/cryptonote/src/Common/StringView.cpp
   ${CRYPTONOTE_LIB}/cryptonote/src/Common/VectorOutputStream.cpp
   ${CRYPTONOTE_LIB}/src/crypto/blake256.c
   ${CRYPTONOTE_LIB}/src/crypto/chacha.c
   ${CRYPTONOTE_LIB}/src/crypto/crypto-ops-data.c
   ${CRYPTONOTE_LIB}/src/crypto/crypto-ops.c
   ${CRYPTONOTE_LIB}/src/crypto/crypto.cpp
   ${CRYPTONOTE_LIB}/src/crypto/groestl.c
   ${CRYPTONOTE_LIB}/src/crypto/hash-extra-blake.c
   ${CRYPTONOTE_LIB}/src/crypto/hash-extra-groestl.c
   ${CRYPTONOTE_LIB}/src/crypto/hash-extra-jh.c
   ${CRYPTONOTE_LIB}/src/crypto/hash-extra-skein.c
   ${CRYPTONOTE_LIB}/src/crypto/hash.c
   ${CRYPTONOTE_LIB}/src/crypto/jh.c
   ${CRYPTONOTE_LIB}/src/crypto/keccak.c
   ${CRYPTONOTE_LIB}/src/crypto/oaes_lib.c
   ${CRYPTONOTE_LIB}/src/crypto/random.c
   ${CRYPTONOTE_LIB}/src/crypto/skein.c
   ${CRYPTONOTE_LIB}/src/crypto/slow-hash.c
   ${CRYPTONOTE_LIB}/src/crypto/slow-hash.cpp
   ${CRYPTONOTE_LIB}/src/crypto/tree-hash.c
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/BlockchainIndices.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/BlockchainMessages.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/BlockIndex.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/CoreConfig.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/CryptoNoteBasic.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/CryptoNoteTools.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/Currency.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/DepositIndex.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/MinerConfig.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/Transaction.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/Account.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/Blockchain.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/Checkpoints.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/Core.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/CryptoNoteSerialization.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/Difficulty.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/IBlock.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/Miner.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/TransactionExtra.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/TransactionPool.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/TransactionPrefixImpl.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteCore/TransactionUtils.cpp
   ${CRYPTONOTE_LIB}/src/InProcessNode/InProcessNode.cpp
   ${CRYPTONOTE_LIB}/src/InProcessNode/InProcessNodeErrors.cpp
   ${CRYPTONOTE_LIB}/src/NodeRpcProxy/NodeErrors.cpp
   ${CRYPTONOTE_LIB}/src/NodeRpcProxy/NodeRpcProxy.cpp
   ${CRYPTONOTE_LIB}/src/P2p/NetNodeConfig.cpp
   ${CRYPTONOTE_LIB}/src/Serialization/BinaryInputStreamSerializer.cpp
   ${CRYPTONOTE_LIB}/src/Serialization/BinaryOutputStreamSerializer.cpp
   ${CRYPTONOTE_LIB}/src/Serialization/JsonInputValueSerializer.cpp
   ${CRYPTONOTE_LIB}/src/Serialization/JsonOutputStreamSerializer.cpp
   ${CRYPTONOTE_LIB}/src/Serialization/KVBinaryInputStreamSerializer.cpp
   ${CRYPTONOTE_LIB}/src/Serialization/KVBinaryOutputStreamSerializer.cpp
   ${CRYPTONOTE_LIB}/src/Serialization/SerializationOverloads.cpp
   ${CRYPTONOTE_LIB}/src/Transfers/BlockchainSynchronizer.cpp
   ${CRYPTONOTE_LIB}/src/Transfers/SynchronizationState.cpp
   ${CRYPTONOTE_LIB}/src/Transfers/TransfersConsumer.cpp
   ${CRYPTONOTE_LIB}/src/Transfers/TransfersContainer.cpp
   ${CRYPTONOTE_LIB}/src/Transfers/TransfersSubscription.cpp
   ${CRYPTONOTE_LIB}/src/Transfers/TransfersSynchronizer.cpp
   ${CRYPTONOTE_LIB}/src/Wallet/LegacyKeysImporter.cpp
   ${CRYPTONOTE_LIB}/src/Wallet/WalletAsyncContextCounter.cpp
   ${CRYPTONOTE_LIB}/src/Wallet/WalletErrors.cpp
   ${CRYPTONOTE_LIB}/src/WalletLegacy/KeysStorage.cpp
   ${CRYPTONOTE_LIB}/src/WalletLegacy/WalletLegacy.cpp
   ${CRYPTONOTE_LIB}/src/WalletLegacy/WalletHelper.cpp
   ${CRYPTONOTE_LIB}/src/WalletLegacy/WalletLegacySerializer.cpp
   ${CRYPTONOTE_LIB}/src/WalletLegacy/WalletLegacySerialization.cpp
   ${CRYPTONOTE_LIB}/src/WalletLegacy/WalletTransactionSender.cpp
   ${CRYPTONOTE_LIB}/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
   ${CRYPTONOTE_LIB}/src/WalletLegacy/WalletUserTransactionsCache.cpp
   ${CRYPTONOTE_LIB}/src/System/ContextGroup.cpp
   ${CRYPTONOTE_LIB}/src/System/Event.cpp
   ${CRYPTONOTE_LIB}/src/System/EventLock.cpp
   ${CRYPTONOTE_LIB}/src/System/InterruptedException.cpp
   ${CRYPTONOTE_LIB}/src/System/Ipv4Address.cpp
   ${CRYPTONOTE_LIB}/src/System/TcpStream.cpp
   ${CRYPTONOTE_LIB}/src/HTTP/HttpRequest.cpp
   ${CRYPTONOTE_LIB}/src/HTTP/HttpParser.cpp
   ${CRYPTONOTE_LIB}/src/HTTP/HttpParserErrorCodes.cpp
   ${CRYPTONOTE_LIB}/src/HTTP/HttpResponse.cpp
   ${CRYPTONOTE_LIB}/src/Rpc/HttpClient.cpp
   ${CRYPTONOTE_LIB}/src/Rpc/JsonRpc.cpp
   ${CRYPTONOTE_LIB}/src/P2p/NetNode.cpp
   ${CRYPTONOTE_LIB}/src/P2p/LevinProtocol.cpp
   ${CRYPTONOTE_LIB}/src/P2p/NetNodeConfig.cpp
   ${CRYPTONOTE_LIB}/src/P2p/PeerListManager.cpp
   ${CRYPTONOTE_LIB}/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
   ${CRYPTONOTE_LIB}/src/Logging/ILogger.cpp
   ${CRYPTONOTE_LIB}/src/Logging/LoggerGroup.cpp
   ${CRYPTONOTE_LIB}/src/Logging/CommonLogger.cpp
   ${CRYPTONOTE_LIB}/src/Logging/LoggerManager.cpp
   ${CRYPTONOTE_LIB}/src/Logging/FileLogger.cpp
   ${CRYPTONOTE_LIB}/src/Logging/StreamLogger.cpp
   ${CRYPTONOTE_LIB}/src/Logging/ConsoleLogger.cpp
   ${CRYPTONOTE_LIB}/src/Logging/LoggerMessage.cpp
   ${CRYPTONOTE_LIB}/src/Logging/LoggerRef.cpp
   ${CRYPTONOTE_LIB}/src/BlockchainExplorer/BlockchainExplorer.cpp
   ${CRYPTONOTE_LIB}/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
   ${CRYPTONOTE_LIB}/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)


set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} external/miniupnpc/minissdpc.c)

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})

  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Cryptonote GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})
	
    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})

set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui Network)

# Installation

set(CPACK_PACKAGE_NAME ${CN_PROJECT_NAME})
set(CPACK_PACKAGE_VERSION ${CN_VERSION})
set(CPACK_PACKAGE_VENDOR "XDN-project")
set(CPACK_PACKAGE_CONTACT "http://digitalnote.org")
set(CPACK_STRIP_FILES ON)
if (APPLE)
    set(CPACK_GENERATOR DragNDrop)
    install(TARGETS ${PROJECT_NAME} BUNDLE DESTINATION .)
    set(CPACK_PACKAGE_FILE_NAME "${CPACK_PACKAGE_NAME}-${CPACK_PACKAGE_VERSION}.-${VERSION_RELEASE}")
elseif (UNIX)
    find_program(RPMBUILD rpmbuild)

    install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION bin)
    install(FILES src/digitalnotewallet.desktop DESTINATION share/applications)
    install(FILES src/images/cryptonote.png DESTINATION share/pixmaps RENAME digitalnote.png)
    install(FILES copyright DESTINATION share/doc/digitalnotewallet)

    if (NOT RPMBUILD)
        set(CPACK_GENERATOR DEB)
        set(CPACK_DEBIAN_PACKAGE_NAME ${CPACK_PACKAGE_NAME})
        set(CPACK_SYSTEM_NAME amd64)
        set(CPACK_DEBIAN_PACKAGE_MAINTAINER "${CPACK_PACKAGE_VENDOR} <xdnproject@tutanota.com>")
        set(CPACK_DEBIAN_PACKAGE_SECTION Office)
        set(CPACK_DEBIAN_PACKAGE_SHLIBDEPS ON)
        set(CPACK_DEBIAN_PACKAGE_DESCRIPTION "DigitalNote XDN wallet
 DigitalNote is a decentrilized privacy protected peer-to-peer
 cryptocurrency and untraceable encrypted messaging system. XDN is
 open-source; its design is public, nobody owns or controls XDN and
 everyone can take part.
 .
 Features of the DigitalNote wallet listed bellow:
 * Classic Bitcoin alike appearance in CryptoNote based cryptocurrency,
   made with QT and open-source.
 * Instant anonymous XDN transfers and transfers notifications
 * Instant untraceable encrypted messages transfers and notifications
 * Safe and secure, full XDN network node in wallet, fast network
   syncronization.
 * Wallet encryption
 * Fast and hardware optimized
 * Open and easy. Private keys can be exported into other clients.")
set(CPACK_PACKAGE_FILE_NAME "${CPACK_DEBIAN_PACKAGE_NAME}-${CPACK_PACKAGE_VERSION}-${VERSION_RELEASE}.${CPACK_SYSTEM_NAME}")
    else()
        set(CPACK_GENERATOR RPM)
        set(CPACK_SYSTEM_NAME x86_64)
        set(CPACK_RPM_PACKAGE_RELEASE ${VERSION_RELEASE})
        set(CPACK_RPM_PACKAGE_LICENSE "MIT")
        set(CPACK_RPM_PACKAGE_GROUP Office)
        set(CPACK_RPM_PACKAGE_REQUIRES "qt5-qtbase >= 5.3.2, qt5-qtbase-gui >= 5.3.2")
        set(CPACK_RPM_PACKAGE_SUMMARY "DigitalNote XDN wallet")
        set(CPACK_RPM_PACKAGE_DESCRIPTION "Open-source DigitalNote XDN GUI wallet
 DigitalNote is a decentrilized privacy protected peer-to-peer
 cryptocurrency and untraceable encrypted messaging system. XDN is
 open-source; its design is public, nobody owns or controls XDN and
 everyone can take part.

 Features of the DigitalNote wallet listed bellow:
 *Classic Bitcoin alike appearance in CryptoNote based cryptocurrency,
    made with QT and open-source.
 *Instant anonymous XDN transfers and transfers notifications
 *Instant untraceable encrypted messages transfers and notifications
 *Safe and secure, full XDN network node in wallet, fast network
    syncronization.
 *Wallet encryption
 *Fast and hardware optimized
 *Open and easy. Private keys can be exported into other clients.")
set(CPACK_PACKAGE_FILE_NAME "${CPACK_PACKAGE_NAME}-${CPACK_PACKAGE_VERSION}-${VERSION_RELEASE}.${CPACK_SYSTEM_NAME}")
    endif ()
elseif (WIN32)
  install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION .)
  set(CPACK_GENERATOR ZIP)
endif (APPLE)

include(CPack)
)
