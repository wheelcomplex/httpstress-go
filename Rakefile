# A script to cross-compile Go project and ZIP the binaries.
# Settings are specified in a YAML file: build.yml.
#
# Example config:
#
# platforms:
#   - name: linux
#     arch:
#       - 386
#       - amd64
#   - name: windows
#     arch: amd64
#   - name: windows
#     arch: 386
# out: ~/Downloads
#
# Please note that if 'out' is specified, ZIP files will appear in the specified directory;
# if not, they will be in current directory.

# Copyright 2014 Chai Chillum
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

require 'yaml'

desc 'Build and ZIP this project for the platforms in build.yml'
task :release do
  config = YAML.load_file 'build.yml'

  host = Hash.new
  host['arch'] = `go env GOHOSTARCH`.to_s.chomp
  host['os'] = `go env GOOS`.chomp

  config['platforms'].each do |os|
    if os['arch'].respond_to?('each')
      os['arch'].each do |arch|
        build os['name'], arch, host, config['out']
      end
    else
      build os['name'], os['arch'], host, config['out']
    end
  end
end

def build os, arch, host, out
  ENV['GOOS'] = os
  ENV['GOARCH'] = arch.to_s
  puts "Building #{os}_#{arch}"

  if system('go install') == true
    pack os, arch, host, out, `go list -f '{{.Target}}'`
  end
end

def pack os, arch, host, out, file
  unless out
    out = '.'
  end

  if host['os'] == os and host['arch'] == arch
    dir = "#{ENV['GOPATH']}/bin"
  else
    dir = "#{ENV['GOPATH']}/bin/#{os}_#{arch}"
  end

  zip = system("zip -qj #{out}/#{os}_#{arch}.zip #{file}")
  if zip == true
    puts "Wrote #{out}/#{os}_#{arch}.zip"
  end
end
