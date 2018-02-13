# -*- coding: utf-8 -*-

#----------------------------------------------------------------------------
#
# Set up the Muhkuh Build System.
#
SConscript('mbs/SConscript')
Import('env_default')

# Create a build environment for the ARM9 based netX chips.
#env_arm9 = env_default.CreateEnvironment(['gcc-arm-none-eabi-4.7', 'asciidoc'])

# Create a build environment for the Cortex-R based netX chips.
env_cortex7 = env_default.CreateEnvironment(['gcc-arm-none-eabi-4.9', 'asciidoc'])


#----------------------------------------------------------------------------
#
# Create the compiler environments.
#
astrIncludePaths = ['src', '#platform/src', '#platform/src/lib', '#targets/version']

env_netx4000_default = env_cortex7.CreateCompilerEnv('4000', ['arch=armv7', 'thumb'], ['arch=armv7-r', 'thumb'])
env_netx4000_default.Append(CPPPATH = astrIncludePaths)
env_netx4000_default.Replace(BOOTBLOCK_CHIPTYPE = 4000)

Export('env_netx4000_default')


#----------------------------------------------------------------------------
#
# Get the source code version from the VCS.
#
env_default.Version('#targets/version/version.h', 'templates/version.h')


#----------------------------------------------------------------------------
#
# Build the platform libraries.
#
PLATFORM_LIB_CFG_BUILDS = [4000]
SConscript('platform/SConscript', exports='PLATFORM_LIB_CFG_BUILDS')
Import('platform_lib_netx4000')


#----------------------------------------------------------------------------
# This is the list of sources. The elements must be separated with whitespace
# (i.e. spaces, tabs, newlines). The amount of whitespace does not matter.
sources = """
	src/netx4000/init.S
"""


#----------------------------------------------------------------------------
#
# Build all files.
#
# netX4000 CR7 for LLRAM
env_netx4000_cr7_llram = env_netx4000_default.Clone()
env_netx4000_cr7_llram.Replace(LDFILE = 'src/netx4000/intram.ld')
src_netx4000_cr7_llram = env_netx4000_cr7_llram.SetBuildPath('targets/netx4000_cr7_llram', 'src', sources)
elf_netx4000_cr7_llram = env_netx4000_cr7_llram.Elf('targets/netx4000_cr7_llram/apply_asic_ctrl_netx4000.elf', src_netx4000_cr7_llram + platform_lib_netx4000)
txt_netx4000_cr7_llram = env_netx4000_cr7_llram.ObjDump('targets/netx4000_cr7_llram/apply_asic_ctrl_netx4000.txt', elf_netx4000_cr7_llram, OBJDUMP_FLAGS=['--disassemble', '--source', '--all-headers', '--wide'])
bin_netx4000_cr7_llram = env_netx4000_cr7_llram.ObjCopy('targets/netx4000_cr7_llram/apply_asic_ctrl_netx4000.bin', elf_netx4000_cr7_llram)
