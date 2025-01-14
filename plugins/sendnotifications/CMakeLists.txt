kdeconnect_add_plugin(kdeconnect_sendnotifications)

target_sources(kdeconnect_sendnotifications PRIVATE
    sendnotificationsplugin.cpp
    notificationslistener.cpp
    notifyingapplication.cpp
)

target_link_libraries(kdeconnect_sendnotifications
    kdeconnectcore
    KF${QT_MAJOR_VERSION}::I18n
    Qt::Gui
    KF${QT_MAJOR_VERSION}::IconThemes
    KF${QT_MAJOR_VERSION}::ConfigCore
)


if(WIN32)
target_sources(kdeconnect_sendnotifications PRIVATE windowsnotificationslistener.cpp)
target_link_libraries(kdeconnect_sendnotifications runtimeobject windowsapp)
else()
target_sources(kdeconnect_sendnotifications PRIVATE dbusnotificationslistener.cpp)
target_link_libraries(kdeconnect_sendnotifications ${DBus_LIBRARIES})
target_include_directories(kdeconnect_sendnotifications
    SYSTEM PRIVATE ${DBus_INCLUDE_DIRS}
)
endif()

# Config
kdeconnect_add_kcm(kdeconnect_sendnotifications_config)

ki18n_wrap_ui(kdeconnect_sendnotifications_config sendnotifications_config.ui)
target_sources(kdeconnect_sendnotifications_config PRIVATE
     sendnotifications_config.cpp
     notifyingapplication.cpp
     notifyingapplicationmodel.cpp
)

target_link_libraries(kdeconnect_sendnotifications_config
    kdeconnectcore
    kdeconnectpluginkcm
    KF${QT_MAJOR_VERSION}::I18n
    KF${QT_MAJOR_VERSION}::KCMUtils
)
