set(lib_name plotctrl)
#######################################
# include directories
include_directories( 
  ../../wxthings/include
  ../include
  )
#######################################
# library sources
########################
# source
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
# headers
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
# library
add_library(${lib_name} STATIC ${${lib_name}_libsrcs})
set_wxxtarget_properties(${lib_name})
#######################################
# install
install(TARGETS ${lib_name} EXPORT ${PROJECT_NAME}-targets
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  )
install(FILES ${plot_hdrs} DESTINATION include/${wxinc}/wx/plotctrl)
install(EXPORT ${PROJECT_NAME}-targets DESTINATION lib/cmake)
