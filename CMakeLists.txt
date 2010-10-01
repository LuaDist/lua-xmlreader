# Copyright (C) 2007-2009 LuaDist.
# Created by Peter Kapec
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

PROJECT(lua_xmlreader C)
CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
INCLUDE(dist.cmake)

# We need Xml2 to compile
FIND_PACKAGE (LibXml2 REQUIRED)
INCLUDE_DIRECTORIES(${LIBXML2_INCLUDE_DIR})

# Build
INCLUDE_DIRECTORIES(.)
ADD_LUA_MODULE(xmlreader error.c xmlreader.c)
TARGET_LINK_LIBRARIES(xmlreader ${LIBXML2_LIBRARIES})
ADD_LUA_TEST(example.lua)

# Build doc
FIND_PROGRAM(LUA NAMES lua lua.bat)
ADD_CUSTOM_COMMAND(
	OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/xmlreader.html
	DEPENDS doc/extract_doc.lua xmlreader.c
	COMMAND ${LUA} doc/extract_doc.lua xmlreader.c
	        > ${CMAKE_CURRENT_BINARY_DIR}/xmlreader.html
	WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)
ADD_CUSTOM_TARGET(html ALL DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/xmlreader.html)

# Install all files and documentation
INSTALL (TARGETS xmlreader DESTINATION ${INSTALL_CMOD})
INSTALL (FILES LICENSE README DESTINATION ${INSTALL_DATA})
INSTALL (FILES example.lua DESTINATION ${INSTALL_TEST})
INSTALL (FILES doc/doc_style.css ${CMAKE_CURRENT_BINARY_DIR}/xmlreader.html DESTINATION ${INSTALL_DOC})
