load("@io_bazel_rules_rust//rust:rust.bzl", "rust_library")

package(default_visibility = ["//visibility:public"])

config_setting(
    name = "use_system_library",
    values = {"cpu": "k8"},
)

rust_library(
    name = "libz_sys",  # Cargo package names can have -'s, but crates cannot.
    srcs = ["src/lib.rs"],
    deps = [
        "@libc//:libc",
    ] + select({
        # On linux we can link to the system installation of zlib.
        ":use_system_library": ["@linux_usr_lib//:z"],
        # Otherwise we can build it.
        "//conditions:default": ["@zlib//:zlib"],
    }),
)

# @TODO Bazel doesn't look like it handles musl easily; rules_rust also doesn't have good linking controls.
