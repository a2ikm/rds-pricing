#!/usr/bin/env rake

task :default => :build

task :build do
  require "csv"

  class InstanceType
    def initialize(data)
      @data = data
    end

    def name
      self["Instance Type"]
    end

    def [](key)
      @data[key]
    end
  end

  root_dir = File.expand_path("..", __FILE__)

  specs = {}
  csv_path = File.join(root_dir, "csv/spec.csv")
  csv = CSV.read(csv_path, headers: true)
  csv.each do |row|
    instance_type = InstanceType.new(row.to_hash)
    next if instance_type.name.nil? || instance_type.name.empty?
    specs[instance_type.name] = instance_type
  end

  build_dir = File.join(root_dir, "build")
  sh "mkdir -p #{build_dir}"

  spec_keys = [
    "DB Instance Class",
    "Instance Type",
    "vCPU",
    "ECU",
    "Memory (GiB)",
    "PIOPS-Optimized",
    "Network Performance",
  ]

  price_keys = [
    "Price Per Hour",
  ]

  Dir[File.join(root_dir, "csv/prices/*.csv")].each do |csv_path|
    prices = {}
    csv = CSV.read(csv_path, headers: true)
    csv.each do |row|
      instance_type = InstanceType.new(row.to_hash)
      prices[instance_type.name] = instance_type
    end

    basename = File.basename(csv_path)
    csv_path = File.join(build_dir, basename)
    CSV.open(csv_path, "w") do |csv|
      csv << [spec_keys, price_keys].flatten
      
      specs.each do |name, spec|
        price = prices[name]
        csv << [
          spec_keys.map { |key| spec[key] },
          price_keys.map { |key| price[key] },
        ].flatten
      end
    end
  end
end
