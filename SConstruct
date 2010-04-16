import platform
import os

cflags='-O2 -pipe -DHAVE_CONFIG_H '
prefix='/usr/local'

slmsource=['src/slm/ids2ngram/ids2ngram.cpp',
            'src/slm/ids2ngram/idngram_merge.cpp',
            'src/slm/mmseg/mmseg.cpp',
            'src/slm/tslminfo/tslminfo.cpp',
            'src/slm/tslmpack/arpa_slm.cpp',
            'src/slm/tslmpack/arpa_conv.cpp',
            'src/slm/tslmpack/slmpack.cpp',
            'src/slm/slm.cpp',
            'src/slm/slminfo/slminfo.cpp',
            'src/slm/sim_sen.cpp',
            'src/slm/sim_slm.cpp',
            'src/slm/getWordFreq/getWordFreq.cpp',
            'src/slm/slmseg/slmseg.cpp',
            'src/slm/thread/slmthread.cpp',
            'src/slm/thread/test_vc.cpp',
            'src/slm/thread/ValueCompress.cpp',
            'src/slm/slmbuild/slmbuild.cpp',
            'src/slm/slmprune/slmprune.cpp',
            'src/slm/sim_slmbuilder.cpp',
            'src/slm/tslmendian/slm_endian.cpp',
            'src/slm/tslmendian/writer.cpp',
            'src/slm/tslmendian/slm_file.cpp',
            'src/slm/sim_dict.cpp',
            'src/portability.cpp',
            'src/lexicon/trie_writer.cpp',
            'src/lexicon/genPYT.cpp',
            'src/lexicon/pytrie_gen.cpp',
            'src/lexicon/pytrie.cpp',
            'src/pinyin/pinyin_data.cpp',
            'src/pinyin/shuangpin_data.cpp',
            'src/pinyin/shuangpin_seg.cpp',
            'src/pinyin/pinyin_seg.cpp']

imesource=['src/portability.cpp',
           'src/slm/slm.cpp',
           'src/lexicon/pytrie.cpp',
           'src/pinyin/pinyin_data.cpp',
           'src/pinyin/pinyin_seg.cpp',
           'src/pinyin/shuangpin_data.cpp',
           'src/pinyin/shuangpin_seg.cpp',
           'src/ime-core/imi_context.cpp',
           'src/ime-core/imi_data.cpp',
           'src/ime-core/lattice_states.cpp',
           'src/ime-core/imi_view.cpp',
           'src/ime-core/imi_uiobjects.cpp',
           'src/ime-core/imi_view_classic.cpp',
           'src/ime-core/imi_winHandler.cpp',
           'src/ime-core/ic_history.cpp',
           'src/ime-core/imi_funcobjs.cpp',
           'src/ime-core/imi_options.cpp',
           'src/ime-core/imi_option_event.cpp',
           'src/ime-core/userdict.cpp']

headers=['src/slm/ids2ngram/idngram.h',
         'src/slm/ids2ngram/idngram_merge.h',
         'src/slm/slm.h',
         'src/slm/tslmpack/arpa_slm.h',
         'src/slm/tslmpack/common.h',
         'src/slm/tslmpack/arpa_conv.h',
         'src/slm/sim_dict.h',
         'src/slm/sim_sen.h',
         'src/slm/sim_slm.h',
         'src/slm/thread/ValueCompress.h',
         'src/slm/sim_fmerge.h',
         'src/slm/sim_slmbuilder.h',
         'src/slm/tslmendian/slm_file.h',
         'src/slm/tslmendian/writer.h',
         'src/lexicon/pytrie_gen.h',
         'src/lexicon/trie_writer.h',
         'src/lexicon/pytrie.h',
         'src/ime-core/imi_view_classic.h',
         'src/ime-core/imi_uiobjects.h',
         'src/ime-core/lattice_states.h',
         'src/ime-core/ic_history.h',
         'src/ime-core/imi_funcobjs.h',
         'src/ime-core/imi_context.h',
         'src/ime-core/imi_winHandler.h',
         'src/ime-core/userdict.h',
         'src/ime-core/imi_option_event.h',
         'src/ime-core/imi_data.h',
         'src/ime-core/utils.h',
         'src/ime-core/imi_keys.h',
         'src/ime-core/imi_option_keys.h',
         'src/ime-core/imi_options.h',
         'src/ime-core/imi_defines.h',
         'src/ime-core/imi_view.h',
         'src/portability.h',
         'src/pinyin/shuangpin_seg.h',
         'src/pinyin/datrie.h',
         'src/pinyin/quanpin_trie.h',
         'src/pinyin/pinyin_seg.h',
         'src/pinyin/pinyin_data.h',
         'src/pinyin/syllable.h',
         'src/pinyin/shuangpin_data.h',
         'src/pinyin/datrie_impl.h',
         'src/host_os.h']

AddOption('--prefix', dest='prefix', type='string', nargs=1, action='store',
          metavar='DIR', help='installation prefix')

if GetOption('prefix') is not None:
    prefix = GetOption('prefix')

cflags += ('-DSUNPINYIN_DATA_DIR=\'"%s/lib/sunpinyin/data"\'' % (prefix,))
libdir = prefix+'/lib'
libdatadir = libdir+'/sunpinyin/data'
headersdir = prefix+'/include/sunpinyin2.0'

#
#==============================environment==============================
#
#
def allinc():
    inc=[]
    for root, dirs, files in os.walk('src'):
        inc.append(root)
    return inc

env = Environment(CFLAGS=cflags,CXXFLAGS=cflags,
                  CPPPATH=['.'] + allinc(), PREFIX=prefix)

#
#==============================configure================================
#
def CheckPKGConfig(context, version='0.12.0'):
    context.Message( 'Checking for pkg-config... ' )
    ret = context.TryAction('pkg-config --atleast-pkgconfig-version=%s' % version)[0]
    context.Result(ret)
    return ret

def CheckPKG(context, name):
    context.Message( 'Checking for %s... ' % name )
    ret = context.TryAction('pkg-config --exists \'%s\'' % name)[0]
    context.Result(ret)
    return ret

conf = Configure(env, custom_tests={'CheckPKGConfig' : CheckPKGConfig,
                                    'CheckPKG' : CheckPKG })

config_h_content = ''

def AddConfigItem(macro_name, res):
    global config_h_content
    config_h_content += ('#define %s %s\n\n' % (macro_name, res))

def AddTestHeader(header):
    macro_name = header.replace('.', '_').replace('-', '_').replace('/', '_').upper()
    macro_name = 'HAVE_' + macro_name
    if conf.CheckCHeader(header):
        AddConfigItem(macro_name, 1)

def AddTestFunction(funcname):
    macro_name = funcname.upper()
    macro_name = 'HAVE_' + macro_name
    if conf.CheckFunc(funcname):
        AddConfigItem(macro_name, 1)

def LinkOSHeader():
    osstring = platform.uname()[0]
    header = ''
    if osstring == 'Linux':
        header = 'linux.h'
    elif osstring == 'SunOS':
        header = 'solaris.h'

    os.system('ln -sf ./config/%s ./src/host_os.h' % (header,));

def DoConfigure():
    if GetOption('clean'):
        return
    
    if not conf.CheckPKGConfig():
        Exit(1)

    if not conf.CheckPKG('sqlite3'):
        Exit(1)
    AddConfigItem('ENABLE_NLS', 1)
    AddConfigItem('GETTEXT_PACKAGE', '"sunpinyin2"')
    AddTestHeader('assert.h')
    AddTestFunction('bind_textdomain_codeset')
    AddTestFunction('dcgettext')
    AddTestHeader('dlfcn.h')
    AddTestFunction('exp2')
    AddTestHeader('fcntl.h')
    AddTestHeader('getopt.h')
    AddTestFunction('getopt_long')
    AddTestFunction('getpagesize')
    AddTestFunction('get_opt')
    AddTestHeader('iconv.h')
    AddTestHeader('inttypes.h')
    AddTestHeader('locale.h')
    AddTestHeader('libintl.h')
    AddTestHeader('limits.h')
    AddTestHeader('locale.h')
    AddTestFunction('log2')
    AddTestHeader('memory.h')
    AddTestFunction('memset')
    AddTestFunction('mmap')
    AddTestFunction('munmap')
    AddTestFunction('setlocale')
    AddTestFunction('strndup')
    AddTestHeader('sys/mman.h')
    AddTestHeader('sys/param.h')
    AddTestHeader('sys/stat.h')
    AddTestHeader('sys/types.h')
    AddTestHeader('unistd.h')
    AddTestHeader('wchar.h')
    AddConfigItem('PACKAGE', '"sunpinyin"')
    AddConfigItem('PACKAGE_NAME', '"sunpinyin"')
    AddConfigItem('PACKAGE_STRING', '"sunpinyin 2.0"')
    AddConfigItem('PACKAGE_TARNAME', '"sunpinyin"')
    AddConfigItem('PACKAGE_VERSION', '"2.0"')
    AddConfigItem('VRESION', '"2.0"')
    f = file('config.h', 'w')
    f.write(config_h_content)
    f.close()
    env = conf.Finish()
    env.ParseConfig('pkg-config sqlite3 --libs --cflags')
    LinkOSHeader()

DoConfigure()

env.Object(slmsource)

SConscript(['build/SConscript'], exports='env')

env.SharedLibrary('sunpinyin', imesource)

env.Command('rawlm', 'build/tslmpack', "make -C raw -f Makefile.new")
env.Command('lm', 'rawlm', "make -C data -f Makefile.new")

if GetOption('clean'):
    os.system('make -C raw -f Makefile.new clean')
    os.system('make -C data -f Makefile.new clean')

def DoInstall():
    lib_target = env.Install(libdir, ['libsunpinyin.so'])
    libdata_target = env.Install(libdatadir,
                                 ['data/lm_sc.t3g',
                                  'data/lm_sc.t3g.le',
                                  'data/pydict_sc.bin.be',
                                  'data/lm_sc.t3g.be',
                                  'data/pydict_sc.bin',
                                  'data/pydict_sc.bin.le'])
    header_targets = []
    for header in headers:
        header_targets.append(env.InstallAs(headersdir + header[3:], header))
    env.Alias('install-headers', header_targets)
    env.Alias('install-lib', lib_target)
    env.Alias('install-libdata', libdata_target)

DoInstall()
env.Alias('install', ['install-lib', 'install-libdata', 'install-headers'])
