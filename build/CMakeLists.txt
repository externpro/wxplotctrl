set(lib_name plotctrl)
#######################################
set(plot_srcs
  ../src/fourier.c
  ../src/fparser.cpp
  ../src/fpoptimizer.cc
  ../src/lm_lsqr.cpp
  ../src/plotctrl.cpp
  ../src/plotcurv.cpp
  ../src/plotdata.cpp
  ../src/plotdraw.cpp
  ../src/plotfunc.cpp
  ../src/plotmark.cpp
  ../src/plotprnt.cpp
  )
list(APPEND ${lib_name}_libsrcs ${plot_srcs})
########################
set(plot_hdrs
  ../include/wx/plotctrl/fourier.h
  ../include/wx/plotctrl/fparser.h
  ../include/wx/plotctrl/lm_lsqr.h
  ../include/wx/plotctrl/plotctrl.h
  ../include/wx/plotctrl/plotcurv.h
  ../include/wx/plotctrl/plotdata.h
  ../include/wx/plotctrl/plotdefs.h
  ../include/wx/plotctrl/plotdraw.h
  ../include/wx/plotctrl/plotfunc.h
  ../include/wx/plotctrl/plotmark.h
  ../include/wx/plotctrl/plotprnt.h
  )
list(APPEND ${lib_name}_libsrcs ${plot_hdrs})
#######################################
if(DEFINED WX_SOURCE)
  message(STATUS "WX_SOURCE: ${WX_SOURCE}") # passed-in via externpro
endif()
#######################################
add_library(${lib_name} STATIC ${${lib_name}_libsrcs})
target_include_directories(${lib_name} PUBLIC $<INSTALL_INTERFACE:$<INSTALL_PREFIX>/${wxInc}>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../include>
  PRIVATE
  # wxplotctrl plotdraw.cpp needs to include wx/gtk/dcclient.h when building against gtk2
  ${WX_SOURCE}/include # TODO should be removed when building against gtk2 becomes obsolete
  )
target_link_libraries(${lib_name} PUBLIC things)
list(TRANSFORM WXX_OUTPUT_NAME REPLACE "@TARGET@" "${lib_name}")
set_target_properties(${lib_name} PROPERTIES OUTPUT_NAME ${WXX_OUTPUT_NAME})
#######################################
install(TARGETS ${lib_name} EXPORT ${targetsFile}
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  )
install(FILES ${plot_hdrs} DESTINATION ${wxInc}/wx/${lib_name})
if(DEFINED XP_NAMESPACE)
  set(nameSpace NAMESPACE ${XP_NAMESPACE}::)
endif()
install(EXPORT ${targetsFile} DESTINATION lib/cmake ${nameSpace})
#######################################
set(samples_plotctrl_srcs
  ../samples/plotctrl/wxplotctrl.cpp
  ../samples/plotctrl/wxplotctrl.rc
  )
foreach(sample
  samples_plotctrl
  )
  add_executable(${sample} ${${sample}_srcs})
  target_link_libraries(${sample} PRIVATE ${lib_name})
endforeach()
