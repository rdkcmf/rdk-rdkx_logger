##########################################################################
# If not stated otherwise in this file or this component's LICENSE
# file the following copyright and licenses apply:
#
# Copyright 2019 RDK Management
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
##########################################################################
include_HEADERS = rdkx_logger.h \
                  rdkx_logger_modules.h

sysconf_DATA = rdkx_logger.json

lib_LTLIBRARIES = librdkx-logger.la
librdkx_logger_la_SOURCES = rdkx_logger_modules.jsonc    \
                            rdkx_logger_level.hash       \
                            rdkx_logger_modules_lookup.c \
                            rdkx_logger.c

# Create perfect hash .c file from .hash files
.hash.c:
	${STAGING_BINDIR_NATIVE}/gperf --output-file=$@ $<


# These files all need something produced by json to c script so have them depend on the .c
rdkx_logger_modules.h:        rdkx_logger_modules.c
rdkx_logger_modules_lookup.c: rdkx_logger_modules.c
rdkx_logger.c:                rdkx_logger_modules.c


if RDKV_ENABLED
# Copy rdkv json file to the same dir as the source code
rdkx_logger_modules.jsonc: ./rdkv/rdkx_logger.json
	cp $< $@
else
# Copy input json file to the same dir as the source code
rdkx_logger_modules.jsonc: ../../rdkx_logger.json
	cp $< $@
endif

rdkx_logger.json: rdkx_logger_modules.jsonc
	cp $< $@

# Process the json file to produce output files *.hash, *.h, *_lookup.c
.jsonc.c: 
	${RDKX_LOGGER_MODULE_TO_C} $< $(basename $<)
	${STAGING_BINDIR_NATIVE}/gperf --output-file=$(basename $<).c $(basename $<).hash

