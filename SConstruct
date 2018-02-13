# -*- coding: utf-8 -*-

#----------------------------------------------------------------------------
#
# Set up the Muhkuh Build System.
#
SConscript('mbs/SConscript')
Import('atEnv')

# Create a build environment for the Cortex-R7 and Cortex-A9 based netX chips.
env_cortexR7 = atEnv.DEFAULT.CreateEnvironment(['gcc-arm-none-eabi-4.9', 'asciidoc'])
env_cortexR7.CreateCompilerEnv('NETX4000', ['arch=armv7', 'thumb'], ['arch=armv7-r', 'thumb'])


#----------------------------------------------------------------------------
#
# Create the compiler environments.
#
astrIncludePaths = ['src', '#platform/src', '#platform/src/lib', '#targets/version']

atEnv.NETX4000.Append(CPPPATH = astrIncludePaths)


#----------------------------------------------------------------------------
#
# Get the source code version from the VCS.
#
atEnv.DEFAULT.Version('#targets/version/version.h', 'templates/version.h')


#----------------------------------------------------------------------------
#
# Build the platform libraries.
#
# Build the platform libraries.
SConscript('platform/SConscript')


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
env_netx4000_cr7_llram = atEnv.NETX4000.Clone()
env_netx4000_cr7_llram.Replace(LDFILE = 'src/netx4000/intram.ld')
src_netx4000_cr7_llram = env_netx4000_cr7_llram.SetBuildPath('targets/netx4000_cr7_llram', 'src', sources)
elf_netx4000_cr7_llram = env_netx4000_cr7_llram.Elf('targets/netx4000_cr7_llram/apply_asic_ctrl_netx4000.elf', src_netx4000_cr7_llram + env_netx4000_cr7_llram['PLATFORM_LIBRARY'])
txt_netx4000_cr7_llram = env_netx4000_cr7_llram.ObjDump('targets/netx4000_cr7_llram/apply_asic_ctrl_netx4000.txt', elf_netx4000_cr7_llram, OBJDUMP_FLAGS=['--disassemble', '--source', '--all-headers', '--wide'])
bin_netx4000_cr7_llram = env_netx4000_cr7_llram.ObjCopy('targets/netx4000_cr7_llram/apply_asic_ctrl_netx4000.bin', elf_netx4000_cr7_llram)
