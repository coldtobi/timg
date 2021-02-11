add_executable(timg)
target_sources(timg PRIVATE
  timg.cc
  image-display.cc
  image-source.cc
  renderer.cc
  terminal-canvas.cc
  display-options.h
  image-display.h
  image-source.h
  renderer.h
  terminal-canvas.h
  thread-pool.h
  timg-time.h
)

target_link_libraries(timg PkgConfig::GRAPHICSMAGICKXX Threads::Threads)
target_include_directories(timg PRIVATE ${CMAKE_BINARY_DIR}/src)

target_compile_features(timg PRIVATE cxx_std_14)

if(TIMG_VERSION_FROM_GIT)
  git_describe(GIT_DESCRIBE_VERSION)
  git_committime(GIT_DATE)
else()
  set(GIT_DESCRIBE_VERSION "${PROJECT_VERSION}+")
endif()

configure_file(timg-version.h.in timg-version.h)

if(WITH_VIDEO_DECODING)
  target_sources(timg PUBLIC video-display.cc video-display.h)
  target_compile_definitions(timg PUBLIC WITH_TIMG_VIDEO)
  target_link_libraries(timg PkgConfig::LIBAV PkgConfig::WSCALE)
endif()

install(TARGETS timg)