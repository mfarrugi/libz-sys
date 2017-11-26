REVISION = "e5ec8bef453fdc496551731c5b82ca9598bed382"

http_archive(
    name = "io_bazel_rules_rust",
    strip_prefix = "rules_rust-" + REVISION,
    urls = [
        "https://github.com/bazelbuild/rules_rust/archive/" + REVISION + ".tar.gz",
    ],
)

load("@io_bazel_rules_rust//rust:repositories.bzl", "rust_repositories")

rust_repositories()

# Provide bazelfied libc.
new_git_repository(
    name = "libc",
    remote = "https://github.com/rust-lang/libc",
    tag = "0.2.20",
    build_file = "bazel/libc.BUILD",
)

# Build and statically link zlib.
new_local_repository(
    name = "zlib",
    path = "src/zlib",
    build_file = "bazel/zlib.BUILD"
)

# Link to the system zlib.
new_local_repository(
    name = "linux_usr_lib",
    # pkg-config --variable=libdir zlib
    path = "/usr/lib/x86_64-linux-gnu",
    build_file_content =
"""
cc_library(
    name = "z",
    # @TODO This should be the .so, which needs https://github.com/bazelbuild/rules_rust/pull/61 pushed.
    srcs = ["libz.a"],
    visibility = ["//visibility:public"],
)
""",
)

