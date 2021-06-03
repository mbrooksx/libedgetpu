# Make uses /bin/sh by default, which is incompatible with the bashisms seen
# below.
SHELL := /bin/bash

ifeq ($(origin MAKEFILE_DIR), undefined)
	MAKEFILE_DIR := $(shell dirname $(realpath $(lastword $(MAKEFILE_LIST))))
endif

BUILDROOT ?= $(MAKEFILE_DIR)/..
BUILDDIR := $(BUILDROOT)/out
TOBUILDDIR = $(addprefix $(BUILDDIR)/,$(1))
MKDIR = if [ ! -d $(dir $@) ]; then mkdir -p $(dir $@); fi
CC=gcc
CXX=g++
FLATC=flatc

# TFROOT ?= /tmp/tensorflow

LIBEDGETPU_CFLAGS += -fPIC
LIBEDGETPU_CFLAGS += -Wall
LIBEDGETPU_CFLAGS += -std=c99

LIBEDGETPU_CXXFLAGS += -fPIC
LIBEDGETPU_CXXFLAGS += -Wall
LIBEDGETPU_CXXFLAGS += -std=c++14
LIBEDGETPU_CXXFLAGS += -DDARWINN_PORT_DEFAULT

LIBEDGETPU_LDFLAGS += -Wl,-Map=$(BUILDDIR)/output.map
LIBEDGETPU_LDFLAGS += -shared
LIBEDGETPU_LDFLAGS += -Wl,--soname,libedgetpu.so.1
LIBEDGETPU_LDFLAGS += -Wl,--version-script=$(BUILDROOT)/tflite/public/libedgetpu.lds
LIBEDGETPU_LDFLAGS += -fuse-ld=gold
LIBEDGETPU_LDFLAGS += -lflatbuffers
LIBEDGETPU_LDFLAGS += -labsl_flags
LIBEDGETPU_LDFLAGS += -labsl_flags_internal
LIBEDGETPU_LDFLAGS += -labsl_flags_reflection
LIBEDGETPU_LDFLAGS += -labsl_flags_marshalling
LIBEDGETPU_LDFLAGS += -labsl_hash
LIBEDGETPU_LDFLAGS += -labsl_hashtablez_sampler
LIBEDGETPU_LDFLAGS += -labsl_raw_hash_set
LIBEDGETPU_LDFLAGS += -labsl_str_format_internal
LIBEDGETPU_LDFLAGS += -lusb-1.0

LIBEDGETPU_FLATC_SRCS += $(BUILDROOT)/executable/executable.fbs
LIBEDGETPU_FLATC_SRCS += $(BUILDROOT)/api/driver_options.fbs
LIBEDGETPU_FLATC_OBJS := $(call TOBUILDDIR,$(patsubst %.fbs,%_generated.h,$(LIBEDGETPU_FLATC_SRCS)))

LIBEDGETPU_FW_SRCS += $(BUILDROOT)/driver/usb/apex_latest_single_ep.bin
LIBEDGETPU_FW_SRCS += $(BUILDROOT)/driver/usb/apex_latest_multi_ep.bin
LIBEDGETPU_FW_OUTPUT = $(call TOBUILDDIR, $(BUILDROOT)/driver/usb/usb_latest_firmware.h)

LIBEDGETPU_INCLUDES += $(BUILDROOT)
LIBEDGETPU_INCLUDES += $(TFROOT)
LIBEDGETPU_INCLUDES += $(BUILDDIR)
LIBEDGETPU_INCLUDES += $(BUILDDIR)/$(BUILDROOT)
LIBEDGETPU_INCLUDES := $(addprefix -I,$(LIBEDGETPU_INCLUDES))

LIBEDGETPU_CSRCS += $(TFROOT)/tensorflow/lite/c/common.c
LIBEDGETPU_COBJS := $(call TOBUILDDIR,$(patsubst %.c,%.o,$(LIBEDGETPU_CSRCS)))

LIBEDGETPU_CCSRCS += $(BUILDROOT)/api/allocated_buffer.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/api/buffer.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/api/driver_options_helper.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/api/layer_information.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/api/tensor_util.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/api/watchdog.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/aligned_allocator.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/allocator.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/beagle/beagle_kernel_top_level_handler.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/beagle/beagle_pci_driver_provider.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/beagle/beagle_pci_driver_provider_linux.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/beagle/beagle_top_level_handler.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/beagle/beagle_top_level_interrupt_manager.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/beagle/beagle_usb_driver_provider.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/device_buffer.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/device_buffer_mapper.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/dma_chunker.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/dma_info.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/dma_info_extractor.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/driver.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/driver_factory.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/driver_factory_default.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/executable_util.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/instruction_buffers.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/interrupt/grouped_interrupt_controller.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/interrupt/interrupt_controller.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/interrupt/top_level_interrupt_manager.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/kernel/kernel_coherent_allocator.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/kernel/kernel_event_handler.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/kernel/kernel_interrupt_handler.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/kernel/kernel_mmu_mapper.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/kernel/kernel_registers.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/kernel/linux/kernel_coherent_allocator_linux.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/kernel/linux/kernel_event_handler_linux.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/kernel/linux/kernel_event_linux.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/kernel/linux/kernel_registers_linux.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/memory/buddy_address_space.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/memory/buddy_allocator.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/memory/dual_address_space.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/memory/mmio_address_space.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/memory/mmu_mapper.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/memory/nop_address_space.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/mmio/coherent_allocator.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/mmio_driver.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/package_registry.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/package_verifier.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/real_time_dma_scheduler.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/registers/registers.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/request.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/run_controller.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/scalar_core_controller.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/single_queue_dma_scheduler.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/single_tpu_request.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/usb/libusb_options_default.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/usb/local_usb_device.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/usb/usb_dfu_commands.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/usb/usb_dfu_util.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/usb/usb_driver.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/usb/usb_io_request.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/usb/usb_ml_commands.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/usb/usb_registers.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver/usb/usb_standard_commands.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/driver_shared/time_stamper/driver_time_stamper.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/port/blocking_counter.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/port/default/port_from_tf/logging.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/port/default/port_from_tf/status.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/port/default/port_from_tf/statusor.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/port/default/stringprintf.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/port/shared_mutex.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/port/timer_portable.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/tflite/custom_op.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/tflite/custom_op_data.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/tflite/custom_op_direct.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/tflite/custom_op_user_data_direct.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/tflite/edgetpu_c.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/tflite/edgetpu_delegate_for_custom_op.cc
LIBEDGETPU_CCSRCS += $(BUILDROOT)/tflite/edgetpu_delegate_for_custom_op_tflite_plugin.cc
LIBEDGETPU_CCSRCS += $(TFROOT)/tensorflow/lite/util.cc
LIBEDGETPU_CCOBJS := $(call TOBUILDDIR,$(patsubst %.cc,%.o,$(LIBEDGETPU_CCSRCS)))

# In order to support direct and throttled mode - we need to compile two files
# twice (changing -DTHROTTLE_EDGE_TPU).
LIBEDGETPU_MAX_CCSRCS += $(BUILDROOT)/tflite/edgetpu_context_direct.cc
LIBEDGETPU_MAX_CCSRCS += $(BUILDROOT)/tflite/edgetpu_manager_direct.cc
LIBEDGETPU_MAX_CCOBJS := $(call TOBUILDDIR,$(patsubst %.cc,%.o,$(LIBEDGETPU_MAX_CCSRCS)))

LIBEDGETPU_STD_CCSRCS += $(BUILDROOT)/tflite/edgetpu_context_direct.cc
LIBEDGETPU_STD_CCSRCS += $(BUILDROOT)/tflite/edgetpu_manager_direct.cc
LIBEDGETPU_STD_CCOBJS := $(call TOBUILDDIR,$(patsubst %.cc,%-throttled.o,$(LIBEDGETPU_STD_CCSRCS)))

.PHONY: libedgetpu

all: libedgetpu libedgetpu-throttled

clean:
	rm -rf $(BUILDDIR)

$(LIBEDGETPU_FLATC_OBJS) : $(BUILDDIR)/%_generated.h: %.fbs
	@$(MKDIR)
	@$(FLATC) --cpp --gen-object-api --force-empty --gen-mutable -o $(dir $@) $<

firmware:
	mkdir -p $(dir $(LIBEDGETPU_FW_OUTPUT))
	@echo "namespace {" > $(LIBEDGETPU_FW_OUTPUT)
	for FILE in $(LIBEDGETPU_FW_SRCS); do \
	  FILENAME=$${FILE##*/} ; \
	  FILEBASE=$${FILENAME%.*} ; \
	  echo "const unsigned char ""$$FILEBASE"" [] = {" >> $(LIBEDGETPU_FW_OUTPUT) ; \
	  xxd -i < "$$FILE" >> $(LIBEDGETPU_FW_OUTPUT) ; \
	  echo "};" >> $(LIBEDGETPU_FW_OUTPUT) ; \
	  echo "constexpr unsigned int ""$$FILEBASE""_len = sizeof(""$$FILEBASE"")/sizeof(unsigned char);" >> $(LIBEDGETPU_FW_OUTPUT) ; \
	  echo "" >> $(LIBEDGETPU_FW_OUTPUT) ; \
	done
	@echo "} // namespace" >> $(LIBEDGETPU_FW_OUTPUT)


$(LIBEDGETPU_COBJS) : $(BUILDDIR)/%.o: %.c
	@$(MKDIR)
	@echo "Compiling $<"
	@$(CC) $(LIBEDGETPU_CFLAGS) $(LIBEDGETPU_INCLUDES) -c $< -MD -MT $@ -MF $(@:%o=%d) -o $@

$(LIBEDGETPU_CCOBJS) : $(BUILDDIR)/%.o: %.cc
	@$(MKDIR)
	@echo "Compiling $<"
	@$(CXX) $(LIBEDGETPU_CXXFLAGS) $(LIBEDGETPU_INCLUDES) -c $< -MD -MT $@ -MF $(@:%o=%d) -o $@

$(LIBEDGETPU_MAX_CCOBJS) : $(BUILDDIR)/%.o: %.cc
	@$(MKDIR)
	@echo "Compiling $<"
	@$(CXX) $(LIBEDGETPU_CXXFLAGS) $(LIBEDGETPU_INCLUDES) -c $< -MD -MT $@ -MF $(@:%o=%d) -o $@

$(LIBEDGETPU_STD_CCOBJS) : $(BUILDDIR)/%-throttled.o: %.cc
	@$(MKDIR)
	@echo "Compiling $<"
	@$(CXX) -DTHROTTLE_EDGE_TPU $(LIBEDGETPU_CXXFLAGS) $(LIBEDGETPU_INCLUDES) -c $< -MD -MT $@ -MF $(@:%o=%d) -o $@

libedgetpu: | firmware $(LIBEDGETPU_FLATC_OBJS) $(LIBEDGETPU_COBJS) $(LIBEDGETPU_CCOBJS) $(LIBEDGETPU_MAX_CCOBJS)
	@mkdir $(BUILDDIR)/direct
	@echo "Building libedgetpu.so"
	@$(CXX) $(LIBEDGETPU_CCFLAGS) $(LIBEDGETPU_LDFLAGS) $(LIBEDGETPU_COBJS) $(LIBEDGETPU_CCOBJS) $(LIBEDGETPU_MAX_CCOBJS) -o $(BUILDDIR)/direct/libedgetpu.so.1
	@ln -sf $(BUILDDIR)/direct/libedgetpu.so.1 $(BUILDDIR)/direct/libedgetpu.so

libedgetpu-throttled: | firmware $(LIBEDGETPU_FLATC_OBJS) $(LIBEDGETPU_COBJS) $(LIBEDGETPU_CCOBJS) $(LIBEDGETPU_STD_CCOBJS)
	@mkdir $(BUILDDIR)/throttled
	@echo "Building throttled libedgetpu.so"
	@$(CXX) $(LIBEDGETPU_CCFLAGS) $(LIBEDGETPU_LDFLAGS) $(LIBEDGETPU_COBJS) $(LIBEDGETPU_CCOBJS) $(LIBEDGETPU_STD_CCOBJS) -o $(BUILDDIR)/throttled/libedgetpu.so.1
	@ln -sf $(BUILDDIR)/throttled/libedgetpu.so.1 $(BUILDDIR)/throttled/libedgetpu.so

