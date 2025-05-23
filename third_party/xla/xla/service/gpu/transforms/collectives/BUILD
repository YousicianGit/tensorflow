load("//xla:xla.bzl", "xla_cc_test")

package(
    # copybara:uncomment default_applicable_licenses = ["//tensorflow:license"],
    default_visibility = [":friends"],
    licenses = ["notice"],
)

package_group(
    name = "friends",
    includes = [
        "//xla:friends",
    ],
)

cc_library(
    name = "convert_async_collectives_to_sync",
    srcs = ["convert_async_collectives_to_sync.cc"],
    hdrs = ["convert_async_collectives_to_sync.h"],
    deps = [
        "//xla/hlo/ir:hlo",
        "//xla/hlo/transforms/collectives:convert_async_collectives_to_sync",
        "//xla/service/gpu:backend_configs_cc",
        "//xla/tsl/platform:errors",
        "//xla/tsl/platform:statusor",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/strings:string_view",
        "@com_google_absl//absl/types:span",
    ],
)

xla_cc_test(
    name = "convert_async_collectives_to_sync_test",
    srcs = ["convert_async_collectives_to_sync_test.cc"],
    deps = [
        ":convert_async_collectives_to_sync",
        "//xla:util",
        "//xla/hlo/ir:hlo",
        "//xla/service/gpu:backend_configs_cc",
        "//xla/tests:hlo_test_base",
        "//xla/tests:xla_internal_test_main",
        "//xla/tsl/lib/core:status_test_util",
        "//xla/tsl/platform:statusor",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/strings:string_view",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "collective_ops_utils",
    srcs = ["collective_ops_utils.cc"],
    hdrs = ["collective_ops_utils.h"],
    deps = [
        "//xla/hlo/ir:hlo",
        "//xla/service/gpu:backend_configs_cc",
    ],
)

cc_library(
    name = "gpu_collective_combiner_utils",
    srcs = ["gpu_collective_combiner_utils.cc"],
    hdrs = ["gpu_collective_combiner_utils.h"],
    deps = [
        ":collective_ops_utils",
        ":convert_async_collectives_to_sync",
        "//xla:util",
        "//xla/hlo/ir:hlo",
        "//xla/hlo/pass:hlo_pass_pipeline",
        "//xla/hlo/utils:hlo_query",
        "//xla/service:collective_ops_utils",
        "//xla/service:collective_utils",
        "//xla/service/gpu:backend_configs_cc",
        "//xla/service/gpu:gpu_hlo_schedule",
        "//xla/stream_executor:device_description",
        "//xla/tsl/platform:errors",
        "@com_google_absl//absl/container:flat_hash_set",
        "@com_google_absl//absl/log",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
    ],
)

xla_cc_test(
    name = "gpu_collective_combiner_utils_test",
    srcs = ["gpu_collective_combiner_utils_test.cc"],
    deps = [
        ":gpu_collective_combiner_utils",
        "//xla:util",
        "//xla/hlo/ir:hlo",
        "//xla/hlo/pass:hlo_pass",
        "//xla/hlo/pass:hlo_pass_pipeline",
        "//xla/hlo/transforms/simplifiers:hlo_dce",
        "//xla/hlo/utils:hlo_query",
        "//xla/service:collective_pipeliner",
        "//xla/service:hlo_module_config",
        "//xla/service/gpu:backend_configs_cc",
        "//xla/stream_executor:device_description",
        "//xla/tests:hlo_test_base",
        "//xla/tsl/platform:status_matchers",
        "//xla/tsl/platform:statusor",
        "@com_google_absl//absl/container:flat_hash_set",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings:string_view",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "gpu_all_gather_combiner",
    srcs = ["all_gather_combiner.cc"],
    hdrs = ["all_gather_combiner.h"],
    deps = [
        ":gpu_collective_combiner_utils",
        "//xla/hlo/ir:hlo",
        "//xla/hlo/pass:hlo_pass",
        "//xla/hlo/transforms/collectives:all_gather_combiner",
        "//xla/service:hlo_domain_map",
        "//xla/service/gpu:backend_configs_cc",
        "//xla/stream_executor:device_description",
        "//xla/tsl/platform:statusor",
        "@com_google_absl//absl/container:flat_hash_set",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings:string_view",
    ],
)

xla_cc_test(
    name =
        "gpu_all_gather_combiner_test",
    srcs = ["all_gather_combiner_test.cc"],
    deps = [
        ":gpu_all_gather_combiner",
        "//xla/hlo/ir:hlo",
        "//xla/hlo/testlib:filecheck",
        "//xla/service:collective_utils",
        "//xla/stream_executor:device_description",
        "//xla/tests:hlo_test_base",
        "@com_google_absl//absl/log",
        "@com_google_absl//absl/strings:string_view",
        "@com_google_googletest//:gtest_main",
        "@local_tsl//tsl/platform:statusor",
        "@local_tsl//tsl/platform:test",
    ],
)

cc_library(
    name = "gpu_reduce_scatter_combiner",
    srcs = ["reduce_scatter_combiner.cc"],
    hdrs = ["reduce_scatter_combiner.h"],
    deps = [
        ":gpu_collective_combiner_utils",
        "//xla/hlo/ir:hlo",
        "//xla/hlo/pass:hlo_pass",
        "//xla/service:hlo_domain_map",
        "//xla/service:reduce_scatter_combiner",
        "//xla/service/gpu:backend_configs_cc",
        "//xla/stream_executor:device_description",
        "//xla/tsl/platform:statusor",
        "@com_google_absl//absl/container:flat_hash_set",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings:string_view",
    ],
)

xla_cc_test(
    name = "reduce_scatter_combiner_test",
    srcs = ["reduce_scatter_combiner_test.cc"],
    deps = [
        ":gpu_reduce_scatter_combiner",
        "//xla/hlo/ir:hlo",
        "//xla/hlo/testlib:filecheck",
        "//xla/service:collective_utils",
        "//xla/stream_executor:device_description",
        "//xla/tests:hlo_test_base",
        "@com_google_absl//absl/log",
        "@com_google_absl//absl/strings:string_view",
        "@com_google_googletest//:gtest_main",
        "@local_tsl//tsl/platform:statusor",
    ],
)

cc_library(
    name = "gpu_all_reduce_combiner",
    srcs = ["all_reduce_combiner.cc"],
    hdrs = ["all_reduce_combiner.h"],
    deps = [
        ":gpu_collective_combiner_utils",
        "//xla/hlo/ir:hlo",
        "//xla/hlo/pass:hlo_pass",
        "//xla/hlo/transforms/collectives:all_reduce_combiner",
        "//xla/service:hlo_domain_map",
        "//xla/service/gpu:backend_configs_cc",
        "//xla/stream_executor:device_description",
        "//xla/tsl/platform:statusor",
        "@com_google_absl//absl/container:flat_hash_set",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings:string_view",
    ],
)

xla_cc_test(
    name = "all_reduce_combiner_test",
    srcs = ["all_reduce_combiner_test.cc"],
    deps = [
        ":gpu_all_reduce_combiner",
        "//xla/hlo/ir:hlo",
        "//xla/hlo/testlib:filecheck",
        "//xla/service:collective_utils",
        "//xla/stream_executor:device_description",
        "//xla/tests:hlo_test_base",
        "@com_google_absl//absl/log",
        "@com_google_absl//absl/strings:string_view",
        "@com_google_googletest//:gtest_main",
        "@local_tsl//tsl/platform:statusor",
    ],
)
