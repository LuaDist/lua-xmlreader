# Copyright (C) 2007-2012 LuaDist.
# Created by Peter Kapec
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

project ( lua_xmlreader C )
cmake_minimum_required ( VERSION 2.8 )
include ( cmake/dist.cmake )
include ( lua )

# We need Xml2 to compile
find_package ( LibXml2 REQUIRED )
include_directories ( ${LIBXML2_INCLUDE_DIR} )

# Build
include_directories ( . )
install_lua_module ( xmlreader error.c xmlreader.c LINK ${LIBXML2_LIBRARIES} )

# Build doc
add_custom_command ( OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/xmlreader.html DEPENDS doc/extract_doc.lua 
  xmlreader.c COMMAND ${LUA_EXECUTABLE} doc/extract_doc.lua xmlreader.c > ${CMAKE_CURRENT_BINARY_DIR}/xmlreader.html 
  WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} )
add_custom_target ( html ALL DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/xmlreader.html )

# Install all files and documentation
install_data ( LICENSE README )
install_example ( example.lua )
install_doc ( doc/doc_style.css ${CMAKE_CURRENT_BINARY_DIR}/xmlreader.html ${INSTALL_DOC} )
