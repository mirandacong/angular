workspace(name = "angular")

#
# Download Bazel toolchain dependencies as needed by build actions
#
http_archive(
    name = "build_bazel_rules_nodejs",
    urls = [
        "http://mirrors.corp.logiocean.com/file-store/rules_nodejs/taobao-registry-0.11.5.zip",
        "https://github.com/bazelbuild/rules_nodejs/archive/0.12.0.zip",
    ],
    strip_prefix = "rules_nodejs-0.12.0",
    sha256 = "2977cdbc8ae0eed7d4186385af56a50a3321a549e2136a959998bba89d2edb6e",
)

http_archive(
    name = "bazel_skylib",
    urls = [
        "http://mirrors.corp.logiocean.com/file-store/bazel-skylib/0.3.1.zip",
        "https://github.com/bazelbuild/bazel-skylib/archive/0.3.1.zip",
    ],
    strip_prefix = "bazel-skylib-0.3.1",
    sha256 = "95518adafc9a2b656667bbf517a952e54ce7f350779d0dd95133db4eb5c27fb1",
)

http_archive(
    name = "io_bazel_rules_webtesting",
    urls = [
        "http://mirrors.corp.logiocean.com/file-store/rules_webtesting/0.2.1.zip",
        "https://github.com/bazelbuild/rules_webtesting/archive/0.2.1.zip",
    ],
    strip_prefix = "rules_webtesting-0.2.1",
    sha256 = "7d490aadff9b5262e5251fa69427ab2ffd1548422467cb9f9e1d110e2c36f0fa",
)

http_archive(
    name = "build_bazel_rules_typescript",
    urls = [
        "http://mirrors.corp.logiocean.com/file-store/rules_typescript/0.16.0.zip",
        "https://github.com/bazelbuild/rules_typescript/archive/0.16.0.zip",
    ],
    strip_prefix = "rules_typescript-0.16.0",
    sha256 = "e65c5639a42e2f6d3f9d2bda62487d6b42734830dda45be1620c3e2b1115070c",
)

http_archive(
    name = "io_bazel_rules_go",
    urls = [
        "http://mirrors.corp.logiocean.com/file-store/rules_go/rules_go-0.10.3.tar.gz",
        "https://github.com/bazelbuild/rules_go/releases/download/0.10.3/rules_go-0.10.3.tar.gz",
    ],
    sha256 = "feba3278c13cde8d67e341a837f69a029f698d7a27ddbb2a202be7a10b22142a",
)

# This commit matches the version of buildifier in angular/ngcontainer
# If you change this, also check if it matches the version in the angular/ngcontainer
# version in /.circleci/config.yml
BAZEL_BUILDTOOLS_VERSION = "82b21607e00913b16fe1c51bec80232d9d6de31c"

http_archive(
    name = "com_github_bazelbuild_buildtools",
    urls = [
        "http://mirrors.corp.logiocean.com/file-store/buildtools/%s.zip" % BAZEL_BUILDTOOLS_VERSION,
        "https://github.com/bazelbuild/buildtools/archive/%s.zip" % BAZEL_BUILDTOOLS_VERSION,
    ],
    strip_prefix = "buildtools-%s" % BAZEL_BUILDTOOLS_VERSION,
    sha256 = "edb24c2f9c55b10a820ec74db0564415c0cf553fa55e9fc709a6332fb6685eff",
)

# Fetching the Bazel source code allows us to compile the Skylark linter
http_archive(
    name = "io_bazel",
    urls = [
        "http://mirrors.corp.logiocean.com/file-store/bazel/968f87900dce45a7af749a965b72dbac51b176b3.zip",
        "https://github.com/bazelbuild/bazel/archive/968f87900dce45a7af749a965b72dbac51b176b3.zip",
    ],
    strip_prefix = "bazel-968f87900dce45a7af749a965b72dbac51b176b3",
    sha256 = "e373d2ae24955c1254c495c9c421c009d88966565c35e4e8444c082cb1f0f48f",
)

# We have a source dependency on the Devkit repository, because it's built with
# Bazel.
# This allows us to edit sources and have the effect appear immediately without
# re-packaging or "npm link"ing.
# Even better, things like aspects will visit the entire graph including
# ts_library rules in the devkit repository.
http_archive(
    name = "angular_cli",
    urls = [
        "http://mirrors.corp.logiocean.com/file-store/angular-cli/v6.1.0-rc.0.zip",
        "https://github.com/angular/angular-cli/archive/v6.1.0-rc.0.zip",
    ],
    strip_prefix = "angular-cli-6.1.0-rc.0",
    sha256 = "8cf320ea58c321e103f39087376feea502f20eaf79c61a4fdb05c7286c8684fd",
)

http_archive(
    name = "org_brotli",
    urls = [
        "http://mirrors.corp.logiocean.com/file-store/brotli/f9b8c02673c576a3e807edbf3a9328e9e7af6d7c.zip",
        "https://github.com/google/brotli/archive/f9b8c02673c576a3e807edbf3a9328e9e7af6d7c.zip",
    ],
    strip_prefix = "brotli-f9b8c02673c576a3e807edbf3a9328e9e7af6d7c",
    sha256 = "8a517806d2b7c8505ba5c53934e7d7c70d341b68ffd268e9044d35b564a48828",
)

#
# Point Bazel to WORKSPACEs that live in subdirectories
#

local_repository(
    name = "rxjs",
    path = "node_modules/rxjs/src",
)

# Point to the integration test workspace just so that Bazel doesn't descend into it
# when expanding the //... pattern
local_repository(
    name = "bazel_integration_test",
    path = "integration/bazel",
)

#
# Load and install our dependencies downloaded above.
#

load("@build_bazel_rules_nodejs//:defs.bzl", "check_bazel_version", "node_repositories", "yarn_install")

check_bazel_version("0.16.0", """
If you are on a Mac and using Homebrew, there is a breaking change to the installation in Bazel 0.16
See https://blog.bazel.build/2018/08/22/bazel-homebrew.html

""")
node_repositories(
  package_json = ["//:package.json"],
  preserve_symlinks = True,
)

load("@io_bazel_rules_go//go:def.bzl", "go_rules_dependencies", "go_register_toolchains")

go_rules_dependencies()
go_register_toolchains()

load("@io_bazel_rules_webtesting//web:repositories.bzl", "browser_repositories", "web_test_repositories")

web_test_repositories()
browser_repositories(
    chromium = True,
    firefox = True,
)

load("@build_bazel_rules_typescript//:defs.bzl", "ts_setup_workspace")

ts_setup_workspace()

load("@angular//:index.bzl", "ng_setup_workspace")

ng_setup_workspace()

#
# Ask Bazel to manage these toolchain dependencies for us.
# Bazel will run `yarn install` when one of these toolchains is requested during
# a build.
#

yarn_install(
    name = "ts-api-guardian_runtime_deps",
    package_json = "//tools/ts-api-guardian:package.json",
    yarn_lock = "//tools/ts-api-guardian:yarn.lock",
)

yarn_install(
    name = "http-server_runtime_deps",
    package_json = "//tools/http-server:package.json",
    yarn_lock = "//tools/http-server:yarn.lock",
)
