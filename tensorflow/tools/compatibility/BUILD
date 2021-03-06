licenses(["notice"])  # Apache 2.0

package(default_visibility = ["//tensorflow:internal"])

load(
    "//tensorflow:tensorflow.bzl",
    "tf_copts",  # @unused
    "tf_cc_test",  # @unused
)

py_library(
    name = "ast_edits",
    srcs = ["ast_edits.py"],
    srcs_version = "PY2AND3",
)

py_test(
    name = "ast_edits_test",
    srcs = ["ast_edits_test.py"],
    srcs_version = "PY2AND3",
    deps = [
        ":ast_edits",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:framework_test_lib",
        "@six_archive//:six",
    ],
)

py_binary(
    name = "tf_upgrade",
    srcs = ["tf_upgrade.py"],
    srcs_version = "PY2AND3",
    deps = [":ast_edits"],
)

py_test(
    name = "tf_upgrade_test",
    srcs = ["tf_upgrade_test.py"],
    srcs_version = "PY2AND3",
    deps = [
        ":tf_upgrade",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:framework_test_lib",
        "@six_archive//:six",
    ],
)

py_library(
    name = "renames_v2",
    srcs = ["renames_v2.py"],
    srcs_version = "PY2AND3",
)

py_binary(
    name = "tf_upgrade_v2",
    srcs = [
        "renames_v2.py",
        "tf_upgrade_v2.py",
    ],
    srcs_version = "PY2AND3",
    deps = [":ast_edits"],
)

py_test(
    name = "tf_upgrade_v2_test",
    srcs = ["tf_upgrade_v2_test.py"],
    srcs_version = "PY2AND3",
    deps = [
        ":tf_upgrade_v2",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:framework_test_lib",
        "@six_archive//:six",
    ],
)

# Keep for reference, this test will succeed in 0.11 but fail in 1.0
# py_test(
#     name = "test_file_v0_11",
#     size = "small",
#     srcs = ["testdata/test_file_v0_11.py"],
#     srcs_version = "PY2AND3",
#     deps = [
#         "//tensorflow:tensorflow_py",
#     ],
# )

genrule(
    name = "generate_upgraded_file",
    testonly = 1,
    srcs = ["testdata/test_file_v0_11.py"],
    outs = [
        "test_file_v1_0.py",
        "report.txt",
    ],
    cmd = ("$(location :tf_upgrade)" +
           " --infile $(location testdata/test_file_v0_11.py)" +
           " --outfile $(location test_file_v1_0.py)" +
           " --reportfile $(location report.txt)"),
    tools = [":tf_upgrade"],
)

py_test(
    name = "test_file_v1_0",
    size = "small",
    srcs = ["test_file_v1_0.py"],
    srcs_version = "PY2AND3",
    deps = [
        "//tensorflow:tensorflow_py",
    ],
)

genrule(
    name = "generate_upgraded_file_v2",
    testonly = 1,
    srcs = ["testdata/test_file_v1_10.py"],
    outs = [
        "test_file_v2_0.py",
        "report_v2.txt",
    ],
    cmd = ("$(location :tf_upgrade_v2)" +
           " --infile $(location testdata/test_file_v1_10.py)" +
           " --outfile $(location test_file_v2_0.py)" +
           " --reportfile $(location report_v2.txt)"),
    tools = [":tf_upgrade_v2"],
)

py_test(
    name = "test_file_v2_0",
    size = "small",
    srcs = ["test_file_v2_0.py"],
    srcs_version = "PY2AND3",
    deps = [
        "//tensorflow:tensorflow_py",
    ],
)

exports_files(
    [
        "ast_edits.py",
        "tf_upgrade.py",
        "renames_v2.py",
        "testdata/test_file_v0_11.py",
        "testdata/test_file_v1_10.py",
    ],
)
