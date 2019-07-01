load(":capnproto.bzl", "cc_capnp_library")

package(default_visibility = ["//visibility:public"])

licenses(["notice"])

cc_library(
    name = "kj",
    srcs = glob(
        [
            "c++/src/kj/**/*.c++",
        ],
        exclude = [
            "c++/src/kj/**/*-test.c++",
            "c++/src/kj/test-helpers.c++",
            "c++/src/kj/test.c++",
        ],
    ),
    hdrs = glob(
        [
            "c++/src/kj/**/*.h",
        ],
        exclude = [
            "c++/src/kj/test.h",
        ],
    ),
    copts = [
        "-Wno-sign-compare",
        "-Wno-strict-aliasing",
        "-Wno-unused-const-variable",
        "-Wno-unused-function",
        "-Wno-user-defined-warnings",
        "-D_WINSOCK_DEPRECATED_NO_WARNINGS",
    ],
    defines = [
        "KJ_USE_FUTEX=0",
    ],
    includes = ["c++/src"],
)

cc_library(
    name = "capnp-lib",
    srcs = glob(
        [
            "c++/src/capnp/**/*.c++",
        ],
        exclude = [
            "c++/src/capnp/compiler/capnp.c++",
            "c++/src/capnp/compiler/capnpc-c++.c++",
            "c++/src/capnp/compiler/capnpc-capnp.c++",
            "c++/src/capnp/test.capnp.h",
            "c++/src/capnp/test-util.c++",
            "c++/src/capnp/**/*-test.c++",
            "c++/src/capnp/**/*-testcase.c++",
        ],
    ),
    hdrs = glob(
        [
            "c++/src/capnp/**/*.h",
        ],
        exclude = [
            "c++/src/capnp/test-util.h",
        ],
    ),
    copts = [
        "-Wno-sign-compare",
    ],
    includes = ["c++/src"],
    linkopts = select({
        "@code8//bzl/crosstool:win32": ["-lAdvAPI32"],
        "@code8//bzl/crosstool:win64": ["-lAdvAPI32"],
        "//conditions:default": [],
    }),
    deps = [":kj"],
)

cc_binary(
    name = "capnp",
    srcs = ["c++/src/capnp/compiler/capnp.c++"],
    deps = [":capnp-lib"],
)

genrule(
    name = "capnpc-bin",
    srcs = [":capnp"],
    outs = ["capnpc"],
    cmd = "ln -s $$PWD/$(location :capnp) $@",
)

cc_binary(
    name = "capnpc-c++",
    srcs = ["c++/src/capnp/compiler/capnpc-c++.c++"],
    copts = [
        "-Wno-enum-compare-switch",
    ],
    deps = [":capnp-lib"],
)

cc_binary(
    name = "capnpc-capnp",
    srcs = ["c++/src/capnp/compiler/capnpc-capnp.c++"],
    deps = [":capnp-lib"],
)

filegroup(
    name = "capnp-capnp",
    srcs = glob([
        "c++/src/capnp/**/*.capnp",
    ]),
)

cc_capnp_library(
    name = "test",
    srcs = ["c++/src/capnp/test.capnp"],
    data = glob(["c++/src/capnp/testdata/**"]),
)

cc_capnp_library(
    name = "test-import",
    srcs = ["c++/src/capnp/test-import.capnp"],
    include = "c++/src/capnp",
    deps = [":test"],
)
