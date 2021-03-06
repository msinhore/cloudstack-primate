// Licensed to the Apache Software Foundation (ASF) under one
// or more contributor license agreements.  See the NOTICE file
// distributed with this work for additional information
// regarding copyright ownership.  The ASF licenses this file
// to you under the Apache License, Version 2.0 (the
// "License"); you may not use this file except in compliance
// with the License.  You may obtain a copy of the License at
//
//   http://www.apache.org/licenses/LICENSE-2.0
//
// Unless required by applicable law or agreed to in writing,
// software distributed under the License is distributed on an
// "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
// KIND, either express or implied.  See the License for the
// specific language governing permissions and limitations
// under the License.

<template>
  <a-spin :spinning="fetchLoading">
    <a-list size="small">
      <a-list-item>
        <div>
          <strong>{{ $t('hypervisorversion') }}</strong>
          <div v-if="host.details">
            {{ host.hypervisor }}
            {{ host.details['Host.OS'] + ' ' + host.details['Host.OS.Version'] }}
          </div>
        </div>
      </a-list-item>
      <a-list-item v-if="host.details && host.details.secured">
        <div>
          <strong>{{ $t('Secured') }}</strong>
          <div>
            {{ host.details.secured }}
          </div>
        </div>
      </a-list-item>
      <a-list-item>
        <div>
          <strong>{{ $t('hosttags') }}</strong>
          <div>
            {{ host.hosttags }}
          </div>
        </div>
      </a-list-item>
      <a-list-item>
        <div>
          <strong>{{ $t('oscategoryid') }}</strong>
          <div>
            {{ host.oscategoryname }}
          </div>
        </div>
      </a-list-item>
      <a-list-item v-if="host.outofbandmanagement">
        <div>
          <strong>{{ $t('OOBM') }}</strong>
          <div>
            {{ host.outofbandmanagement.enabled }}
          </div>
        </div>
      </a-list-item>
      <a-list-item v-if="host.outofbandmanagement">
        <div>
          <strong>{{ $t('powerstate') }}</strong>
          <div>
            {{ host.outofbandmanagement.powerstate }}
          </div>
        </div>
      </a-list-item>
      <a-list-item v-if="host.hostha">
        <div>
          <strong>{{ $t('haenable') }}</strong>
          <div>
            {{ host.hostha.haenable }}
          </div>
        </div>
      </a-list-item>
      <a-list-item v-if="host.hostha">
        <div>
          <strong>{{ $t('hastate') }}</strong>
          <div>
            {{ host.hostha.hastate }}
          </div>
        </div>
      </a-list-item>
      <a-list-item v-if="host.hostha">
        <div>
          <strong>{{ $t('haprovider') }}</strong>
          <div>
            {{ host.hostha.haprovider }}
          </div>
        </div>
      </a-list-item>

    </a-list>
  </a-spin>
</template>

<script>
import { api } from '@/api'

export default {
  name: 'HostInfoTab',
  props: {
    resource: {
      type: Object,
      required: true
    },
    loading: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      host: {},
      fetchLoading: false
    }
  },
  mounted () {
    this.fetchData()
  },
  watch: {
    loading (newData, oldData) {
      if (!newData && this.resource.id) {
        this.fetchData()
      }
    }
  },
  methods: {
    fetchData () {
      this.dataSource = []
      this.itemCount = 0
      this.fetchLoading = true
      api('listHosts', { id: this.resource.id }).then(json => {
        this.host = json.listhostsresponse.host[0]
      }).catch(error => {
        this.$notification.error({
          message: 'Request Failed',
          description: error.response.headers['x-description']
        })
      }).finally(() => {
        this.fetchLoading = false
      })
    }
  }
}
</script>

<style lang="less" scoped>

</style>
