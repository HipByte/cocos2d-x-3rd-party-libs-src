SRC_DIR = Dir.pwd
BIN_DIR = File.expand_path(File.join(SRC_DIR, '../cocos2d-x-3rd-party-libs-bin'))

desc "Build libraries for tvOS"
task :build do
  sh "cd build && ./build.sh --platform=tvos --libs=chipmunk,curl,freetype,jpeg,lua,png,tiff,webp,websockets --arch=all --mode=release"
end

desc "Install libraries"
task :install_libs do
  Dir.chdir('build/tvos') do
    libs = Dir.glob('**/prebuilt/*.a')

    libs.each do |lib|
      lib =~ /^([^\/]+)\//
      dir = $1
      dir = "freetype2" if dir == "freetype"
      dir = "curl" if dir == "crypto"
      dir = "curl" if dir == "ssl"

      if File.exist?(File.join(BIN_DIR, dir))
        install_dir = File.join(BIN_DIR, dir, "prebuilt/tvos")
        FileUtils.mkdir_p(install_dir)
        puts "Copy #{lib} to #{install_dir}"
        FileUtils.cp(lib, install_dir)
      end
    end
  end
end

desc "Archive libraries"
task :archive do
  archive_name = "cocos2d-x-3rd-party-libs-bin-3-deps-78"
  release_name = "v3-deps-78"

  Dir.chdir(BIN_DIR) do
    sh "git archive HEAD --prefix=#{archive_name}/ --output=../#{archive_name}.zip"
    sh "mv ../#{archive_name}.zip ../#{release_name}.zip"
  end
end

task :default => [:build]
