load('//:buckaroo_macros.bzl', 'buckaroo_cell')
load('//:utils.bzl', 'extract')

# The cell name of the Protobuf package
protobuf_cell = buckaroo_cell('github.com/buckaroo-pm/protobuf')

# The Buck target for protoc, the Protobuf compiler
protoc = protobuf_cell + '//:protoc'

# The Buck target for the Protobuf library
protobuf = protobuf_cell + '//:protobuf'

# Generate C++ code from addressbook.proto
genrule(
  name = 'addressbook-cpp',
  out = 'out',
  srcs = [
    'addressbook.proto',
  ],
  cmd = 'mkdir -p $OUT && $(exe ' + protoc + ') $SRCDIR/addressbook.proto --proto_path=$SRCDIR --cpp_out=$OUT',
)

# Group the generated code into a library
cxx_library(
  name = 'addressbook',
  header_namespace = '',
  exported_headers = {
    'addressbook.pb.h': extract(':addressbook-cpp', 'addressbook.pb.h'),
  },
  srcs = [
    extract(':addressbook-cpp', 'addressbook.pb.cc'),
  ],
  deps = [
    protobuf,
  ],
)

# Tool for adding a person to the database
cxx_binary(
  name = 'add-person',
  srcs = [
    'add_person.cc',
  ],
  deps = [
    ':addressbook',
  ],
)

# Tool for listing people in the database
cxx_binary(
  name = 'list-people',
  srcs = [
    'list_people.cc',
  ],
  deps = [
    ':addressbook',
  ],
)
