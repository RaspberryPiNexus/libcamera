# SPDX-License-Identifier: CC0-1.0

if not get_option('test')
    test_enabled = false
    subdir_done()
endif

test_enabled = true

# When ASan is enabled, find the path to the ASan runtime needed by multiple
# tests. This currently works with gcc only, as clang uses different file names
# depending on the compiler version and target architecture.
asan_enabled = false
asan_runtime_missing = false

if get_option('b_sanitize').contains('address')
    asan_enabled = true

    if cc.get_id() == 'gcc'
        asan_runtime = run_command(cc, '-print-file-name=libasan.so', check : true).stdout().strip()
    else
        asan_runtime_missing = true
    endif
endif

subdir('libtest')

subdir('camera')
subdir('controls')
subdir('gstreamer')
subdir('ipa')
subdir('ipc')
subdir('log')
subdir('media_device')
subdir('process')
subdir('py')
subdir('serialization')
subdir('stream')
subdir('v4l2_compat')
subdir('v4l2_subdevice')
subdir('v4l2_videodevice')

public_tests = [
    {'name': 'color-space', 'sources': ['color-space.cpp']},
    {'name': 'geometry', 'sources': ['geometry.cpp']},
    {'name': 'public-api', 'sources': ['public-api.cpp']},
    {'name': 'signal', 'sources': ['signal.cpp']},
    {'name': 'span', 'sources': ['span.cpp']},
    {'name': 'transform', 'sources': ['transform.cpp']},
]

internal_tests = [
    {'name': 'bayer-format', 'sources': ['bayer-format.cpp']},
    {'name': 'byte-stream-buffer', 'sources': ['byte-stream-buffer.cpp']},
    {'name': 'camera-sensor', 'sources': ['camera-sensor.cpp']},
    {'name': 'delayed_controls', 'sources': ['delayed_controls.cpp']},
    {'name': 'event', 'sources': ['event.cpp']},
    {'name': 'event-dispatcher', 'sources': ['event-dispatcher.cpp']},
    {'name': 'event-thread', 'sources': ['event-thread.cpp']},
    {'name': 'file', 'sources': ['file.cpp']},
    {'name': 'flags', 'sources': ['flags.cpp']},
    {'name': 'hotplug-cameras', 'sources': ['hotplug-cameras.cpp']},
    {'name': 'message', 'sources': ['message.cpp']},
    {'name': 'object', 'sources': ['object.cpp']},
    {'name': 'object-delete', 'sources': ['object-delete.cpp']},
    {'name': 'object-invoke', 'sources': ['object-invoke.cpp']},
    {'name': 'pixel-format', 'sources': ['pixel-format.cpp']},
    {'name': 'shared-fd', 'sources': ['shared-fd.cpp']},
    {'name': 'signal-threads', 'sources': ['signal-threads.cpp']},
    {'name': 'threads', 'sources': 'threads.cpp', 'dependencies': [libthreads]},
    {'name': 'timer', 'sources': ['timer.cpp']},
    {'name': 'timer-fail', 'sources': ['timer-fail.cpp'], 'should_fail': true},
    {'name': 'timer-thread', 'sources': ['timer-thread.cpp']},
    {'name': 'unique-fd', 'sources': ['unique-fd.cpp']},
    {'name': 'utils', 'sources': ['utils.cpp']},
    {'name': 'vector', 'sources': ['vector.cpp']},
    {'name': 'yaml-parser', 'sources': ['yaml-parser.cpp']},
]

internal_non_parallel_tests = [
    {'name': 'fence', 'sources': ['fence.cpp']},
    {'name': 'mapped-buffer', 'sources': ['mapped-buffer.cpp']},
]

foreach test : public_tests
    deps = [libcamera_public]
    if 'dependencies' in test
        deps += test['dependencies']
    endif

    exe = executable(test['name'], test['sources'],
                     dependencies : deps,
                     implicit_include_directories : false,
                     link_with : test_libraries,
                     include_directories : test_includes_public)

    test(test['name'], exe, should_fail : test.get('should_fail', false))
endforeach

foreach test : internal_tests
    deps = [libcamera_private]
    if 'dependencies' in test
        deps += test['dependencies']
    endif

    exe = executable(test['name'], test['sources'],
                     dependencies : deps,
                     implicit_include_directories : false,
                     link_with : test_libraries,
                     include_directories : test_includes_internal)

    test(test['name'], exe, should_fail : test.get('should_fail', false))
endforeach

foreach test : internal_non_parallel_tests
    deps = [libcamera_private]
    if 'dependencies' in test
        deps += test['dependencies']
    endif

    exe = executable(test['name'], test['sources'],
                     dependencies : deps,
                     implicit_include_directories : false,
                     link_with : test_libraries,
                     include_directories : test_includes_internal)

    test(test['name'], exe,
         is_parallel : false,
         should_fail : test.get('should_fail', false))
endforeach
