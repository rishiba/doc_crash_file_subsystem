#################
SuperBlock Object
#################

*   We will start by looking at the super block of the mounted file system.

*   Run the mount command to get the list of the mounted file system. Crash will also give the address of the superblock object of the corresponding file system.

*   SuperBlock object keep the metadata related to the whole file system. It
    keeps the information related to the whole file system.

Exercises
=========


#.  See the file system specific super block.

#.  See the file system related information like free inodes, inodes count, magic number. 


*   Following information is maintained in the superblock object.

::

    crash> mount
         MOUNT           SUPERBLK     TYPE   DEVNAME   DIRNAME
    ffff8801162ec000 ffff880116810800 rootfs rootfs    /         
    ffff8801162ed200 ffff8800d57d8000 sysfs  sysfs     /sys      

    >>>>>>>>>>>>>> SNIPPED <<<<<<<<<<<<<<<<<<<<<

    ffff880035760d80 ffff8800d83d2800 fusectl fusectl  /sys/fs/fuse/connections
    ffff880115c66c00 ffff8800d967c000 fuse   lxcfs     /var/lib/lxcfs
    ffff8800da8e9e00 ffff8800dad85800 tmpfs  tmpfs     /run/user/1000
    ffff8800da8e8a80 ffff8800dad84000 ext4   /dev/loop0 /mnt      

*   Let us now read the superblock object.

::

    crash> struct super_block ffff8800dad84000 
    struct super_block {
      s_list = {
        next = 0xffffffff81e79650 <super_blocks>, 
        prev = 0xffff8800dad85800
      }, 
      s_dev = 7340032, 
      s_blocksize_bits = 10 '\n', 
      s_blocksize = 1024, 
      s_maxbytes = 4398046510080, 
      s_type = 0xffffffff81e89f40 <ext4_fs_type>, 
      s_op = 0xffffffff81a364a0 <ext4_sops>, 
      dq_op = 0xffffffff81a365e0 <ext4_quota_operations>, 
      s_qcop = 0xffffffff81a36580 <ext4_qctl_operations>, 
      s_export_op = 0xffffffff81a36440 <ext4_export_ops>, 
      s_flags = 1879113728, 
      s_iflags = 1, 
      >> s_magic = 61267,  // This is in decimal, convert it to HEX to check the magic number.
      s_root = 0xffff8800db2446c0, 
      s_uuid = "\343h\253E\"\371C\204\254<\215]\357l", <incomplete sequence \360>, 
    
      >>>>>>>>>>> SNIPPED <<<<<<<<<<<<
      >>>>>>>>>>> SNIPPED <<<<<<<<<<<<

      s_fs_info = 0xffff8800dad81800, 
      s_max_links = 0, 
      s_mode = 131, 
      s_time_gran = 1000000000, 

*   Let us go to the ext4_sb_info structure which keeps the file system specific information. We can get the address from the VFS super block object.   

::

    crash> struct super_block ffff8800dad84000 | grep fs_info 
      s_fs_info = 0xffff8800dad81800, 

*   Let us read the structure. Note the values starting with ``>>``

::

    crash> struct ext4_sb_info 0xffff8800dad81800, 
    struct ext4_sb_info {
      s_desc_size = 32, 
    >>    s_inodes_per_block = 8, 
    >>    s_blocks_per_group = 8192, 
    >>    s_clusters_per_group = 8192, 
    >>    s_inodes_per_group = 1976, 
      s_itb_per_group = 247, 
      s_gdb_count = 1, 
      s_desc_per_block = 32, 
      s_groups_count = 13, 
      s_blockfile_groups = 13, 
      s_overhead = 7346, 
      s_cluster_ratio = 1, 
      s_cluster_bits = 0, 
      s_bitmap_maxbytes = 17247252480, 
      s_sbh = 0xffff88005db2b9c0, 
      s_es = 0xffff8800d9f73400, 
      s_group_desc = 0xffff8800d83f8850, 
      s_mount_opt = 2818754576, 
      s_mount_opt2 = 2, 
      s_mount_flags = 0, 
      s_def_mount_opt = 2818752528, 
      s_sb_block = 1, 
      s_resv_clusters = {
        counter = 2048
      }, 
      s_resuid = {
        val = 0
      }, 
      s_resgid = {
        val = 0
      }, 
      s_mount_state = 1, 
      s_pad = 0, 
      s_addr_per_block_bits = 8, 
      s_desc_per_block_bits = 5, 
      >> s_inode_size = 128, 
      >> s_first_ino = 11, 
      s_inode_readahead_blks = 32, 
      s_inode_goal = 0, 
      s_next_gen_lock = {
        {
          rlock = {
            raw_lock = {
              val = {
                counter = 0
              }
            }
          }
        }
      }, 
      s_next_generation = 599397868, 
      s_hash_seed = {2954919371, 3477536816, 375261849, 3703976637}, 
      s_def_hash_version = 1, 
      s_hash_unsigned = 0, 
      s_freeclusters_counter = {
        lock = {
          raw_lock = {
            val = {
              counter = 0
            }
          }
        }, 
        count = 93504, 
        list = {
          next = 0xffff8800dad81c90, 
          prev = 0xffff8800dad81918
        }, 
        counters = 0x60fee4801a94
      }, 
      s_freeinodes_counter = {
        lock = {
          raw_lock = {
            val = {
              counter = 0
            }
          }
        }, 
        count = 25677, 
        list = {
          next = 0xffff8800dad818f0, 
          prev = 0xffff8800dad81940
        }, 
        counters = 0x60fee4802508
      }, 
      s_dirs_counter = {
        lock = {
          raw_lock = {
            val = {
              counter = 0
            }
          }
        }, 
        count = 2, 
        list = {
          next = 0xffff8800dad81918, 
          prev = 0xffff8800dad81968
        }, 
        counters = 0x60fee480250c
      }, 
      s_dirtyclusters_counter = {
        lock = {
          raw_lock = {
            val = {
              counter = 0
            }
          }
        }, 
        count = 0, 
        list = {
          next = 0xffff8800dad81940, 
          prev = 0xffffffff81eac440 <percpu_counters>
        }, 
        counters = 0x60fee48025c8
      }, 
      s_blockgroup_lock = 0xffff8800dbbe0000, 
      s_proc = 0xffff8800d584d9c0, 
      s_kobj = {
        name = 0xffff8800d83f88b0 "loop0", 
        entry = {
          next = 0xffffffff81e93680 <ext4_kset>, 
          prev = 0xffff8800d57dd198
        }, 
        parent = 0xffffffff81e93698 <ext4_kset+24>, 
        kset = 0xffffffff81e93680 <ext4_kset>, 
        ktype = 0xffffffff81e93720 <ext4_sb_ktype>, 
        sd = 0xffff8800d886a7f8, 
        kref = {
          refcount = {
            counter = 1
          }
        }, 
        state_initialized = 1, 
        state_in_sysfs = 1, 
        state_add_uevent_sent = 0, 
        state_remove_uevent_sent = 0, 
        uevent_suppress = 0
      }, 
      s_kobj_unregister = {
        done = 0, 
        wait = {
          lock = {
            {
              rlock = {
                raw_lock = {
                  val = {
                    counter = 0
                  }
                }
              }
            }
          }, 
          task_list = {
            next = 0xffff8800dad819e0, 
            prev = 0xffff8800dad819e0
          }
        }
      }, 
      s_sb = 0xffff8800dad84000, 
      s_journal = 0xffff8800dad82000, 
      s_orphan = {
        next = 0xffff8800dad81a00, 
        prev = 0xffff8800dad81a00
      }, 
      s_orphan_lock = {
        count = {
          counter = 1
        }, 
        wait_lock = {
          {
            rlock = {
              raw_lock = {
                val = {
                  counter = 0
                }
              }
            }
          }
        }, 
        wait_list = {
          next = 0xffff8800dad81a18, 
          prev = 0xffff8800dad81a18
        }, 
        owner = 0x0, 
        osq = {
          tail = {
            counter = 0
          }
        }
      }, 
      s_resize_flags = 0, 
      s_commit_interval = 1250, 
      s_max_batch_time = 15000, 
      s_min_batch_time = 0, 
      journal_bdev = 0x0, 
      s_qf_names = {0x0, 0x0}, 
      s_jquota_fmt = 0, 
      s_want_extra_isize = 0, 
      system_blks = {
        rb_node = 0xffff8801168a2f28
      }, 
      s_group_info = 0xffff8800d83f8870, 
      s_buddy_cache = 0xffff88005d8695d8, 
      s_md_lock = {
        {
          rlock = {
            raw_lock = {
              val = {
                counter = 0
              }
            }
          }
        }
      }, 
      s_mb_offsets = 0xffff8800d9175440, 
      s_mb_maxs = 0xffff8800da1fae00, 
      s_group_info_size = 1, 
      s_stripe = 0, 
      s_mb_stream_request = 16, 
      s_mb_max_to_scan = 200, 
      s_mb_min_to_scan = 10, 
      s_mb_stats = 0, 
      s_mb_order2_reqs = 2, 
      s_mb_group_prealloc = 512, 
      s_max_dir_size_kb = 0, 
      s_mb_last_group = 0, 
      s_mb_last_start = 0, 
      s_bal_reqs = {
        counter = 0
      }, 
      s_bal_success = {
        counter = 0
      }, 
      s_bal_allocated = {
        counter = 0
      }, 
      s_bal_ex_scanned = {
        counter = 0
      }, 
      s_bal_goals = {
        counter = 0
      }, 
      s_bal_breaks = {
        counter = 0
      }, 
      s_bal_2orders = {
        counter = 0
      }, 
      s_bal_lock = {
        {
          rlock = {
            raw_lock = {
              val = {
                counter = 0
              }
            }
          }
        }
      }, 
      s_mb_buddies_generated = 0, 
      s_mb_generation_time = 0, 
      s_mb_lost_chunks = {
        counter = 0
      }, 
      s_mb_preallocated = {
        counter = 0
      }, 
      s_mb_discarded = {
        counter = 0
      }, 
      s_lock_busy = {
        counter = 0
      }, 
      s_locality_groups = 0x60fee4804238, 
      s_sectors_written_start = 425836, 
      s_kbytes_written = 4444, 
      s_extent_max_zeroout_kb = 32, 
      s_log_groups_per_flex = 4, 
      s_flex_groups = 0xffff8800dac3df50, 
      s_flex_groups_allocated = 1, 
      rsv_conversion_wq = 0xffff8800d5f73000, 
      s_err_report = {
        entry = {
          next = 0x0, 
          pprev = 0x0
        }, 
        expires = 0, 
        function = 0xffffffff812bc690 <print_daily_error_info>, 
        data = 18446612135985823744, 
        flags = 0, 
        slack = -1, 
        start_pid = -1, 
        start_site = 0x0, 
        start_comm = "\000\000\000\000\000\000\000\000\000\000\000\000\000\000\000"
      }, 
      s_li_request = 0x0, 
      s_li_wait_mult = 10, 
      s_mmp_tsk = 0x0, 
      s_last_trim_minblks = {
        counter = 0
      }, 
      s_chksum_driver = 0x0, 
      s_csum_seed = 0, 
      s_es_shrinker = {
        count_objects = 0xffffffff812de910 <ext4_es_count>, 
        scan_objects = 0xffffffff812df420 <ext4_es_scan>, 
        seeks = 2, 
        batch = 0, 
        flags = 0, 
        list = {
          next = 0xffffffff81e67fd0 <shrinker_list>, 
          prev = 0xffff8800dad844e0
        }, 
        nr_deferred = 0xffff8800d83f8880
      }, 
      s_es_list = {
        next = 0xffff88005d868ff8, 
        prev = 0xffff88005d869428
      }, 
      s_es_nr_inode = 2, 
      s_es_stats = {
        es_stats_shrunk = 0, 
        es_stats_cache_hits = 1, 
        es_stats_cache_misses = 5, 
        es_stats_scan_time = 0, 
        es_stats_max_scan_time = 0, 
        es_stats_all_cnt = {
          lock = {
            raw_lock = {
              val = {
                counter = 0
              }
            }
          }, 
          count = 0, 
          list = {
            next = 0xffff8800d587daf0, 
            prev = 0xffff8800dad81c90
          }, 
          counters = 0x60fee4801a8c
        }, 
        es_stats_shk_cnt = {
          lock = {
            raw_lock = {
              val = {
                counter = 0
              }
            }
          }, 
          count = 0, 
          list = {
            next = 0xffff8800dad81c68, 
            prev = 0xffff8800dad818f0
          }, 
          counters = 0x60fee4801a90
        }
      }, 
      s_mb_cache = 0xffff8800da1fa8c0, 
      s_es_lock = {
        {
          rlock = {
            raw_lock = {
              val = {
                counter = 0
              }
            }
          }
        }
      }, 
      s_err_ratelimit_state = {
        lock = {
          raw_lock = {
            val = {
              counter = 0
            }
          }
        }, 
        interval = 1250, 
        burst = 10, 
        printed = 0, 
        missed = 0, 
        begin = 0
      }, 
      s_warning_ratelimit_state = {
        lock = {
          raw_lock = {
            val = {
              counter = 0
            }
          }
        }, 
        interval = 1250, 
        burst = 10, 
        printed = 0, 
        missed = 0, 
        begin = 0
      }, 
      s_msg_ratelimit_state = {
        lock = {
          raw_lock = {
            val = {
              counter = 0
            }
          }
        }, 
        interval = 1250, 
        burst = 10, 
        printed = 0, 
        missed = 0, 
        begin = 0
      }
    }
    crash> 



*   
::

    sudo tune2fs -l /dev/loop0 
    tune2fs 1.42.13 (17-May-2015)
    Filesystem volume name:   <none>
    Last mounted on:          <not available>
    Filesystem UUID:          e368ab45-22f9-4384-ac3c-8d5def6c4af0
    >> Filesystem magic number:  0xEF53
    Filesystem revision #:    1 (dynamic)
    Filesystem features:      has_journal ext_attr resize_inode dir_index filetype needs_recovery extent flex_bg sparse_super large_file huge_file uninit_bg dir_nlink extra_isize
    Filesystem flags:         signed_directory_hash 
    Default mount options:    user_xattr acl
    Filesystem state:         clean
    Errors behavior:          Continue
    Filesystem OS type:       Linux
    >> Inode count:              25688
    Block count:              102400
    Reserved block count:     5120
    Free blocks:              93504
    >> Free inodes:              25677
    First block:              1
    Block size:               1024
    Fragment size:            1024
    Reserved GDT blocks:      256
    >> Blocks per group:         8192
    Fragments per group:      8192
    >> Inodes per group:         1976
    >> Inode blocks per group:   247
    Flex block group size:    16
    Filesystem created:       Thu Aug 17 05:52:27 2017
    Last mount time:          Thu Aug 17 05:52:40 2017
    Last write time:          Thu Aug 17 05:52:40 2017
    Mount count:              1
    Maximum mount count:      -1
    Last checked:             Thu Aug 17 05:52:27 2017
    Check interval:           0 (<none>)
    Lifetime writes:          4444 kB
    Reserved blocks uid:      0 (user root)
    Reserved blocks gid:      0 (group root)
    >> First inode:              11
    >> Inode size:            128
    Journal inode:            8
    Default directory hash:   half_md4
    Directory Hash Seed:      cb7d20b0-3000-47cf-990a-5e16bd32c6dc
    Journal backup:           inode blocks
