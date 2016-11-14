cmake_minimum_required (VERSION 2.8.11)
project ("comms_all_protocols" NONE)

include (ExternalProject)

set (COMMON_BRANCH "develop")

set (INSTALL_DIR "${CMAKE_BINARY_DIR}/install")
set (COMMS_CHAMPION_PROJ "comms_champion")
set (COMMS_CHAMPION_DIR "${CMAKE_BINARY_DIR}/${COMMS_CHAMPION_PROJ}")
set (COMMS_CHAMPION_SRC_DIR "${COMMS_CHAMPION_DIR}/src")
set (COMMS_CHAMPION_BIN_DIR "${COMMS_CHAMPION_DIR}/build")

if (NOT "${COMMS_ALL_QT_DIR}" STREQUAL "")
    set (extra_comms_champion_args "-DCC_QT_DIR=${COMMS_ALL_QT_DIR}")
    set (extra_mqtt_args "-DMQTT_QT_DIR=${COMMS_ALL_QT_DIR}")
    set (extra_ublox_args "-DUBLOX_QT_DIR=${COMMS_ALL_QT_DIR}")
endif ()

ExternalProject_Add(
    "${COMMS_CHAMPION_PROJ}"
    PREFIX "${COMMS_CHAMPION_BIN_DIR}"
    STAMP_DIR "${COMMS_CHAMPION_BIN_DIR}"    
    GIT_REPOSITORY "https://github.com/arobenko/comms_champion.git"
    GIT_TAG "${COMMON_BRANCH}"
    SOURCE_DIR "${COMMS_CHAMPION_SRC_DIR}"
    CMAKE_ARGS -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE} -DCC_INSTALL_DIR=${INSTALL_DIR} ${extra_comms_champion_args}
    BINARY_DIR "${COMMS_CHAMPION_BIN_DIR}"
    TEST_BEFORE_INSTALL 1
)

set (MQTT_PROJ "mqtt")
set (MQTT_DIR "${CMAKE_BINARY_DIR}/${MQTT_PROJ}")
set (MQTT_SRC_DIR "${MQTT_DIR}/src")
set (MQTT_BIN_DIR "${MQTT_DIR}/build")

ExternalProject_Add(
    "${MQTT_PROJ}"
    DEPENDS "${COMMS_CHAMPION_PROJ}"
    PREFIX "${MQTT_BIN_DIR}"
    STAMP_DIR "${MQTT_BIN_DIR}"    
    GIT_REPOSITORY "https://github.com/arobenko/mqtt.git"
    GIT_TAG "${COMMON_BRANCH}"
    SOURCE_DIR "${MQTT_SRC_DIR}"
    CMAKE_ARGS -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE} -DCC_MQTT_INSTALL_DIR=${INSTALL_DIR} ${extra_mqtt_args}
    BINARY_DIR "${MQTT_BIN_DIR}"
)

set (UBLOX_PROJ "ublox")
set (UBLOX_DIR "${CMAKE_BINARY_DIR}/${UBLOX_PROJ}")
set (UBLOX_SRC_DIR "${UBLOX_DIR}/src")
set (UBLOX_BIN_DIR "${UBLOX_DIR}/build")

ExternalProject_Add(
    "${UBLOX_PROJ}"
    DEPENDS "${COMMS_CHAMPION_PROJ}"
    PREFIX "${UBLOX_BIN_DIR}"
    STAMP_DIR "${UBLOX_BIN_DIR}"    
    GIT_REPOSITORY "https://github.com/arobenko/ublox.git"
    GIT_TAG "${COMMON_BRANCH}"
    SOURCE_DIR "${UBLOX_SRC_DIR}"
    CMAKE_ARGS -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE} -DCC_UBLOX_INSTALL_DIR=${INSTALL_DIR} ${extra_ublox_args}
    BINARY_DIR "${UBLOX_BIN_DIR}"
)

