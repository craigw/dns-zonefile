require 'rake'
require 'spec/rake/spectask'

desc "Run the tests"
Spec::Rake::SpecTask.new('spec') do |t|
  t.spec_files = FileList['spec/**/*_spec.rb']
  t.ruby_opts = [ '-rubygems' ]
  t.spec_opts = [ '--format specdoc' ]
  t.libs << 'lib'
  t.libs << 'spec'
end
task :default => [ :generate_grammar, :spec ]

desc "Regenerate the grammar definition"
task :generate_grammar do
  parser_file = 'lib/dns/zonefile_parser.rb'
  File.unlink(parser_file) if File.exists?(parser_file)
  puts %x[tt doc/zonefile -o #{parser_file}]
  source = "require 'treetop'\n\n"
  source += "module DNS\n"
  parser_source = File.open(parser_file, 'r').read
  parser_source.gsub!(/(\s+)Zonefile$/, '\1ZonefileGrammar # :nodoc:')
  source += parser_source.split(/\n/).map { |l| "\t#{l}" }.join("\n")
  source += "\nend"
  File.open(parser_file, 'w') do |f|
    f.write(source)
  end
end

task :gem => :generate_grammar do
  puts %x[gem build dns-zonefile.gemspec]
end