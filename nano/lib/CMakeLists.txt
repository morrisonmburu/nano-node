if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
	set (platform_sources plat/default/priority.cpp plat/posix/perms.cpp plat/darwin/thread_role.cpp plat/default/debugging.cpp)
elseif (${CMAKE_SYSTEM_NAME} MATCHES "Windows")
	set (platform_sources plat/windows/priority.cpp plat/windows/perms.cpp plat/windows/registry.cpp plat/windows/thread_role.cpp plat/default/debugging.cpp)
elseif (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
	set (platform_sources plat/linux/priority.cpp plat/posix/perms.cpp plat/linux/thread_role.cpp plat/linux/debugging.cpp)
elseif (${CMAKE_SYSTEM_NAME} MATCHES "FreeBSD")
	set (platform_sources plat/default/priority.cpp plat/posix/perms.cpp plat/freebsd/thread_role.cpp plat/plat/default/debugging.cpp)
else ()
	error ("Unknown platform: ${CMAKE_SYSTEM_NAME}")
endif ()

add_library (nano_lib
	${platform_sources}
	alarm.hpp
	alarm.cpp
	asio.hpp
	asio.cpp
	blockbuilders.hpp
	blockbuilders.cpp
	blocks.hpp
	blocks.cpp
	config.hpp
	config.cpp
	configbase.hpp
	diagnosticsconfig.hpp
	diagnosticsconfig.cpp
	errors.hpp
	errors.cpp
	ipc.hpp
	ipc.cpp
	ipc_client.hpp
	ipc_client.cpp
	json_error_response.hpp
	jsonconfig.hpp
	jsonconfig.cpp
	locks.hpp
	locks.cpp
	logger_mt.hpp
	memory.hpp
	memory.cpp
	numbers.hpp
	numbers.cpp
	rep_weights.hpp
	rep_weights.cpp
	rocksdbconfig.hpp
	rocksdbconfig.cpp
	rpc_handler_interface.hpp
	rpcconfig.hpp
	rpcconfig.cpp
	stats.hpp
	stats.cpp
	stream.hpp
	threading.hpp
	threading.cpp
	timer.hpp
	timer.cpp
	tomlconfig.hpp
	tomlconfig.cpp
	utility.hpp
	utility.cpp
	walletconfig.hpp
	walletconfig.cpp
	work.hpp
	work.cpp
	worker.hpp
	worker.cpp)

target_link_libraries (nano_lib
	ed25519
	crypto_lib
	blake2
	ipc_flatbuffers_lib
	${CRYPTOPP_LIBRARY}
	${CMAKE_DL_LIBS}
	Boost::boost)

if (NANO_STACKTRACE_BACKTRACE)
	target_link_libraries(nano_lib backtrace)
endif ()

target_compile_definitions(nano_lib
	PRIVATE
		-DMAJOR_VERSION_STRING=${CPACK_PACKAGE_VERSION_MAJOR}
		-DMINOR_VERSION_STRING=${CPACK_PACKAGE_VERSION_MINOR}
		-DPATCH_VERSION_STRING=${CPACK_PACKAGE_VERSION_PATCH}
		-DPRE_RELEASE_VERSION_STRING=${CPACK_PACKAGE_VERSION_PRE_RELEASE}
	PUBLIC
		-DACTIVE_NETWORK=${ACTIVE_NETWORK}
)
