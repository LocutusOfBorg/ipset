from checklib import *

package = 'ipset'

AddOption('--prefix',
          dest='prefix',
          type='string',
          nargs=1,
          action='store',
          metavar='DIR',
          default='/usr',
          help='Installation prefix')

prefix = GetOption('prefix')
bin = '%s/bin' % prefix
include = '%s/include' % prefix
lib = '%s/lib' % prefix

root_env = Environment(PREFIX = prefix,
                       BINDIR = bin,
                       LIBDIR = lib,
                       INCLUDEDIR = include)

root_env.MergeFlags('-g -O2 -Wall -Werror')

# Only run the configuration steps if we're actually going to build
# something.

if not GetOption('clean') and not GetOption('help'):
    conf = root_env.Configure(custom_tests=
                              {
                               'CheckLibInPath': CheckLibInPath,
                              })

    if not conf.CheckCC():
        print "!! Your compiler and/or environment is not properly configured."
        Exit(0)


    if not conf.CheckLibInPath("buddy",
                               library="libbdd",
                               call="bdd_init(1000, 1000)",
                               header="#include <bdd.h>"):
        print "!! Cannot find the buddy library."
        Exit(0)

    root_env = conf.Finish()

# Include the subdirectory SConscripts

Export('root_env')
SConscript([
            'include/SConscript',
            'src/SConscript',
           ])
